
## VibeX RAG 기반 CS 이메일 자동화 봇 구축
<img width="1165" height="233" alt="Image" src="https://github.com/user-attachments/assets/dda874f1-c886-4073-b1f7-9ef9e12592ca" />

<br>


## 1. 개요

- **목적**: VibeX 공식 고객지원 이메일(`support@vibe-x.app`)로 인입되는 고객 문의를 AI가 자동으로 분석하여, 사내 지식베이스(FAQ, 매뉴얼, 약관 등) 기반의 정확한 답변을 24시간 즉각적으로 회신하는 자동화 파이프라인 구축
- **사용 기술**: n8n (Self-hosted), VibeX RAG Document Search API, **Google Gemini (n8n 내장 LLM 노드)**, Gmail IMAP/SMTP, Prompt Engineering
- **운영 환경**: 자체 서버 (`218.145.67.52:5678`) 백그라운드 상시 구동
    

<br>

## 2. 시스템 아키텍처 및 워크플로우
<img width="1221" height="414" alt="Image" src="https://github.com/user-attachments/assets/c146125c-91b0-4614-8419-eeea40d64956" />
본 시스템은 n8n을 활용하여 총 5단계의 노드(Node) 파이프라인으로 구성되었습니다.


1. **Email Trigger (IMAP)**: `support@vibe-x.app` 계정의 수신 메일을 실시간으로 감지하여 데이터를 추출합니다.
    
2. **Filter**: 마케팅 메일, 시스템 자동 발송(noreply), 스팸 등을 필터링하여 실제 고객의 문의 메일만 다음 단계로 통과시킵니다.
    
3. **VibeX RAG API1 (HTTP Request)**: VibeX의 자체 문서 검색 API(`POST /api/v1/documents/search`)를 호출합니다. 고객의 질문을 전달하여 벡터 DB에 저장된 관련 사내 지식 문서(텍스트 조각)만을 빠르고 정확하게 추출합니다.
    
4. **Basic LLM Chain (Google Gemini)**: 앞서 검색된 지식 문서 내용과 고객의 원본 메일을 조합하여 프롬프트를 구성한 뒤, n8n에 연결된 Gemini LLM 모델을 통해 최종 이메일 답변 텍스트를 생성합니다.
    
5. **Send an Email (SMTP)**: AI가 생성한 최종 답변을 고객에게 발송합니다. Gmail의 앱 비밀번호와 `smtp.gmail.com`을 활용하여 발송과 동시에 `support@vibe-x.app`의 '보낸메일함'에 기록이 자동 동기화되도록 구성했습니다.
    

<br>

## 3. 핵심 구현 기능
- CS 직원이 응대하는 것과 같은 톤앤매너를 유지하기 위해 프롬프트 엔지니어링이 적용되었습니다.


### VibeX 문서 검색 API Payload

``` JSON
{{
  {
    "query": $('Email Trigger (IMAP)').item.json.subject + " \n " + $('Email Trigger (IMAP)').item.json.textPlain,
    "maxResults": 5,
    "minScore": 0.4
  }
}}
```


<br>

### LLM Prompt Engineering

Plaintext

```
당신은 VibeX의 전문적인 이메일 고객 지원 AI입니다. 아래 [고객 문의 내용]의 언어를 분석하고, 처음부터 끝까지 **반드시 문의와 100% 동일한 언어로 번역하여** 답변하세요.

[고객 문의 내용]
메일 제목: {{ $('Email Trigger (IMAP)').item.json.subject }}
메일 본문: {{ $('Email Trigger (IMAP)').item.json.textPlain }}

[검색된 사내 지식 문서]
{{ $json.data && $json.data.contents && $json.data.contents.length > 0 ? $json.data.contents.map(c => c.text).join('\n') : '검색된 지식 문서 없음' }}
```

<br>

### 3-State 상태 관리
``` JSON
"[이메일 3단 구조 작성 가이드]\n"
    "1. 공통 인사말 (필수): 모든 상황에서 이메일 첫 줄은 정중한 줄글 형태의 인사말
    "2. 상태별 본문 (1개만 선택): 아래 세 가지 상태 중 하나만 선택하여 자연스러운 줄글로 작성
        "  상태 1 (지식베이스에서 명확한 답변을 찾은 경우)"
        "  상태 2 (VibeX 서비스나 기술 질문이지만 지식베이스에 없는 경우):"
        "  상태 3 (VibeX와 전혀 무관한 일상 질문인 경우)"
```


- **상태 1 (지식베이스 답변)**: RAG를 통해 검색된 정책 및 매뉴얼 정보(예: 요금제 스펙, 크레딧 복구 정책 등)에 기반하여 정확히 답변합니다.
	
- **상태 2 (기술/계정 확인 필요)**: 지식베이스에 없는 민감한 문의의 경우, 억지로 답변을 지어내지 않고 **"이용에 불편을 드려 대단히 죄송합니다. 담당 부서의 추가적 확인이 필요한 부분입니다."** 라며 정중히 이관합니다.
	
- **상태 3 (스몰토크)**: 서비스와 무관한 일상 질문에는 센스 있는 맞장구로 대응하여 고객 경험을 향상시킵니다.
        

<br>

### 다국어 감지
<img width="1175" height="432" alt="Image" src="https://github.com/user-attachments/assets/7b8703f1-b943-4b1e-bd1c-9eca03bcb004" />

- 고객이 영어, 일본어, 스페인어 등으로 문의할 경우, 정책 문서가 한국어이더라도 반드시 고객의 언어로 번역하여 답변을 생성합니다.


### 이메일 포맷 강제
``` JSON
"[포맷 규칙]"
    "1. 순수 텍스트(Plain Text)만 사용: 이메일 전체에서 마크다운 문법 사용 금지"
    "2. 자연스러운 문장 작성: 쉼표와 접속사를 사용, 자연스럽게 이어지는 하나의 문장으로 풀어 쓸것"
    "3. 기호 사용 금지: 대괄호나 등호(=) 같은 특수 기호로 문단 구분 금지"
    "4. 3단 구조 엄수: 답변은 인사말, 본문, 맺음말의 순서로 작성"
    "[이메일 구조 작성 가이드]"
         "1. 공통 인사말 : 안녕하세요, VibeX 고객지원팀입니다."
         "2. 공통 맺음말 : "'더 자세한 논의나 지원이 필요하시다면 언제든 https://vibe-x.app/contact 로 남겨주세요. 저희 담당 팀이 꼼꼼히 확인 후 도와드리겠습니다.'"
```

-  AI 특유의 마크다운 기호(`*`, `-`, `#`) 출력을 원천 차단하고, **[공통 인사말 ➔ 상태별 본문 ➔ 공통 맺음말 및 Contact 링크]**의 3단 줄글 구조를 엄격히 지키도록 제어했습니다.
    

<br>   

## 4. 기대 효과

- **CS 리소스 절감 및 응답 속도 극대화**: 단순 반복되는 1차 문의를 AI가 24시간 즉각 처리함으로써, CS담당자는 `상태 2`로 분류된 심화 기술 지원에만 집중할 수 있습니다.
    
- **다국어 CS 역량 확보**: 별도의 현지화 인력 없이도 글로벌 고객의 문의에 유창하게 대응할 수 있습니다.
    
