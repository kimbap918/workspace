# [A to Z] vibeX AI 고객센터 이메일 자동화 구축 가이드


<br>

## 0단계: n8n 실행 및 대시보드 접속

- 자동화 툴인 n8n을 실행하고 작업 화면(캔버스)을 띄우는 단계

1. **n8n 실행**: 로컬 PC의 터미널(CMD)을 열고 `npx n8n` 명령어를 입력
    
2. **대시보드 접속**:  `http://localhost:5678`을 입력하여 접속. (서버 대시보드: http://218.145.67.52:5678)
    
3. **새 워크플로우 생성**: n8n 대시보드에 로그인한 뒤, 좌측 메뉴에서 **Workflows**를 클릭하고 우측 상단의 **[Add workflow]**를 눌러 캔버스 준비
    


<br>


## 1단계: Google Cloud API 및 권한(OAuth) 설정

- n8n이 vibeX의 공식 지메일(`support@vibe-x.app`)을 읽고 쓸 수 있도록 구글에서 허락(인증 키)을 받아오는 과정

1. **구글 클라우드 콘솔 접속**: `support@vibe-x.app` 구글 계정으로 [Google Cloud Console](https://console.cloud.google.com/)접속
    
2. **새 프로젝트 만들기**: 상단 `프로젝트 선택 메뉴`를 눌러 **[새 프로젝트]**를 생성
    
3. **Gmail API 활성화**: **API 및 서비스 > 라이브러리**로 이동한 뒤, `Gmail API`를 검색하고 **[사용]** 버튼을 클릭
    
4. **OAuth 동의 화면 설정**: 왼쪽 메뉴의 **API 및 서비스 > OAuth 동의 화면**으로 이동
    
    - User Type을 **내부(Internal)**로 선택하고 **[만들기]**를 클릭
        
    - 앱 이름(예: VibeX), 사용자 지원 이메일 등 필수 항목만 채우고 맨 끝까지 **[저장하고 계속]**을 클릭
        
5. **사용자 인증 정보(키) 생성**: 왼쪽 메뉴에서 **사용자 인증 정보** 탭으로 이동
    
    - 상단의 **[+ 사용자 인증 정보 만들기] > [OAuth 클라이언트 ID]**를 선택
        
    - 애플리케이션 유형을 **웹 애플리케이션(Web application)**으로 선택
        
6. **리디렉션 URI 입력**: 스크롤을 내려 **승인된 리디렉션 URI** 항목에 n8n 주소 `http://localhost:5678/rest/oauth2-credential/callback`를 복사해서 붙여넣고 **[만들기]** 클릭
    
7. **n8n에 키 연동하기**:
    
    - n8n 화면으로 돌아와 Gmail 계정 연결(Credential) 창 이동
        
    - 구글에서 발급받은 **클라이언트 ID(Client ID)**와 **클라이언트 보안 비밀(Client Secret)** 값을 복사하여 n8n 빈칸에 각각 삽입. 
8. **최종 로그인**: n8n 설정 창 맨 아래의 **[Sign in with Google]** 버튼을 클릭하고 구글 로그인 창이 뜨면 권한을 허용하여 연동을 마무리
    

<br>

## 2단계: n8n 노드(파이프라인) 세팅

캔버스에 총 3개의 노드를 구성하여 '메일 수신 ➔ AI 초안 작성 ➔ 임시보관함 저장' 흐름 생성

<br>

### ① 첫 번째 노드: Gmail Trigger (이메일 수신 감지)

- **목적**: 쓸데없는 시스템 메일과 스팸을 거르고, 진짜 고객의 문의 메일만 가져옴
    
- **설정값**:
    
    - **Event**: Message Received
        
    - **Filters > Search**: 아래 명령어를 복사하여 한 줄로 붙여넣기 (사내 메일, 자동 발송, 스팸, SNS 알림 완벽 차단)
        
        `to:support@vibe-x.app -from:vibe-x.app -from:noreply -from:no-reply -category:promotions -category:social`
        

<br>

### ② 두 번째 노드: HTTP Request (VibeX RAG API 호출)

- **목적**: 수신된 메일 본문을 AI에게 전달하고, vibeX 정책 문서에 기반한 '환각 없는' 깔끔한 본문을 생성
    
- **설정값**:
    
    - **Method**: POST
        
    - **URL**: `https://aiagent.vibe-x.app/api/v1/chat`
        
    - **Authentication/Headers**: API 키 및 인증 토큰 입력 (`Content-Type`, `X-API-KEY`, `Authorization`)
        
    - **Body (JSON)**: 아래 코드를 그대로 삽입
        

JSON

```
{
  "message": "고객 이메일 문의입니다. 검색된 VibeX 정책 문서를 바탕으로 친절한 답변 메일 본문만 작성해주세요.\n\n[고객 문의 내용]\n- 메일 제목: {{ $('Gmail Trigger').item.json.subject }}\n- 메일 본문: {{ $json.textPlain }}\n\n[작성 규칙 (환각 방지)]\n1. 정보의 정확성: 반드시 검색된 문서(Context)에 명시된 사실에만 기반하여 답변하세요.\n2. 연락처 생성 절대 금지: 전화번호, 이메일 주소, 웹사이트 링크 등을 절대 임의로 지어내거나 포함하지 마세요.\n3. 포맷 준수: 첫 문장은 반드시 '안녕하세요 고객님,' 으로 시작하세요. 인사말과 핵심 답변 본문만 작성하고, 맺음말(감사합니다 등)은 쓰지 마세요.",
  "provider": "openai",
  "ragEnabled": true,
  "ragMaxResults": 5,
  "ragMinScore": 0.1
}
```


<br>

### ③ 세 번째 노드: Gmail (Create Draft Reply / 임시보관함 저장)

- **목적**: AI가 쓴 글에 고정 꼬리말을 붙여 담당자의 '임시보관함(Drafts)'에 답장 형태로 저장
    
- **설정값**:
    
    - **Resource**: Message / **Operation**: Create Draft (또는 Reply)
        
    - **Add option > To (수신자 추출)**: 원래 발신자의 이메일 주소만 정확히 뽑아내기 위해 아래 수식 삽입
        
        `{{ $('Gmail Trigger').item.json.from.value[0].address }}`
        
    - **Message (메일 본문)**: 아래 텍스트를 그대로 복사/붙혀넣기 
        


```
{{ $json.data.message }}

더 자세한 사항은 문의하기 ( https://stg.vibe-x.app/contact )를 이용해 주시기 바랍니다.

감사합니다.
vibeX 지원팀 드림

---
* 본 초안은 vibeX AI Agent에 의해 자동 작성되었습니다. 검토 후 발송해주세요.
```


<br>


## 3단계: 사전 작업 및 테스트 가동

AI가 엉뚱한 기존 메일(구글 보안 알림 등)을 읽어오지 않도록 메일함을 한 번 정리하고 실제 테스트를 진행

1. **메일함 정리 (필수)**: `support@vibe-x.app` 지메일에 접속하여 가장 최근에 온 **'안 읽은 메일(Unread)'**들을 모두 '읽음' 처리하거나 삭제
    
2. **테스트 메일 발송**: 외부 개인 메일(네이버, 카카오 등)에서 `support@vibe-x.app`으로 테스트 문의를 1통 발송 
    
3. **워크플로우 테스트**: n8n 하단 중앙의 **[Test Workflow]**를 클릭하여 초록색 체크(✅)가 3개 모두 뜨는지 확인
    
4. **결과 확인**: `support@vibe-x.app`의 '임시보관함(Drafts)'에 완벽한 초안이 생성되었는지 확인

5. **자동화 스위치 ON**: 모든 것이 완벽하다면 n8n 우측 상단의 토글 스위치를 **[Publish]**로 전환. 이제 n8n이 24시간 백그라운드에서 동작
    

