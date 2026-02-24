## 도입: Vibench 소개 

- **기존 바이브 코딩 비교:** **체감, 인상** 중심
- **문제:** 프로그램과 제작물을 평가하는 평가자의 주관이 크게 반영
    
- **해결:** 객관적인 평가가 가능한 항목을 통해 평가자의 주관적 해석을 배제하고, 객관적인 지표를 확보하기 위해 전용 벤치마크 시스템 및 커뮤니티 홈페이지 제작


<br>
<br>
<br>


## 무엇을 평가하는가: 난이도 및 채점 항목
<br>

### 1. 프롬프트 난이도 레벨 (PDL)
- 평가 전, 입력된 프롬프트를 L1, L2, L3로 분류하여 채점의 기준점(Anchor)을 다르게 적용. 
- PDL은 **A-4 Logic**, **B-1 CodeQuality**, **B-2 GenEfficiency** 평가 기준에 반영.

- **L1 — Landing/소개형**
    - 키워드 예: introduce, showcase, landing, portfolio, just introduce
    - 섹션 구성/내비/탭·필터/제품 상세(모달·섹션) 중심 소개 경험을 기대함. (목표 E2EDepth: 1)
        
- **L2 — App/CRUD형**
    - 키워드 예: dashboard, admin, CRUD, save/edit/delete, database, API, search+filter+sort
    - 데이터 생성·수정·삭제, 폼 검증, 상태관리, 에러 처리를 기대함. (목표 E2EDepth: 2)
        
- **L3 — Service/E2E형**
    - 키워드 예: auth/login, role/permission, payment/checkout, booking, subscription, onboarding
    - 인증·권한·결제/완료 상태 포함 다단계 워크플로우를 기대함. (목표 E2EDepth: 4)


<br>
<br>


### 2. 3대 평가 섹터

#### **A. Core Performance (핵심 성능 - 60%)**

> **목적:** 사용자의 의도를 단순 코드가 아닌 “시장 출시가 가능한 프로덕트”로 구현하는 능력을 평가

$$A_w = 0.15 \text{Context} + 0.10 \text{MultiModal} + 0.15 \text{Local} + 0.20 \text{Logic} + 0.20 \text{Layout} + 0.20 \text{Depth}$$

#### **B. Developer Experience (개발 및 운영 효율 - 30%)**

> **목적:** 깊이 연동형 생성 속도 보정, 운영 효율, 비용 대비 산출물 완성도를 평가

$$B_w = 0.40 \text{Quality} + 0.40 \text{Efficiency} + 0.12 \text{Cost} + 0.08 \text{SelfHealing}$$

#### **C. Brand Readiness (브랜드 준비도 - 10%)**

> **목적:** 일관된 디자인 시스템과 브랜드 아이덴티티를 갖췄는지 평가

$$C_w = 0.40 \text{Identity} + 0.35 \text{Consistency} + 0.25 \text{Assets}$$

<br>
<br>

## 어떻게 계산하는가: 채점 방식
<br>

### 1. 앵커(Anchor) 시스템
- 모든 세부 항목은 1점(Critical Failure)부터 5점(Strict Perfect)까지의 기준표에 따라 채점
    

> - **1점 (Critical Failure):** 실행이 불가능하거나, 요구사항과 전혀 다른 결과물로 판정됨.
> - **2점 (Below Standard):** 실행은 되지만 로직이 오작동하거나, UI가 심각하게 깨진 상태로 확인됨.
> - **3점 (Base Standard):** 핵심 기능이 정상 동작하며 에러가 없는 상태로 평가됨.
> - **4점 (Generous Good):** 3점 기준을 충족하며 UX가 매끄럽거나 코드가 깔끔한 경우로 판단됨.
> - **5점 (Strict Perfect):** 단순 구현을 넘어 높은 수준의 디테일이 증명된 경우로 평가됨.
>     
> - **Note:** Evidence(증거)가 없으면 점수는 인정되지 않음. 증거 확보가 불가능한 경우 보수적으로 채점하거나 ‘미확인’ 처리


<br>

### 2. 총점 산정식 
- 산출된 각 섹터의 가중 평균을 통해 다음과 같이 0~100점 스케일의 결과를 도출

$$\textbf{Total Score}=(A_w\times12)+(B_w\times6)+(C_w\times2)$$
- $A_w$: 핵심 성능(Core Performance, 60%) 가중평균(0~5)
- $B_w$: 개발 효율(Developer Experience & Efficiency, 30%) 가중평균(0~5)
- $C_w$: 브랜드(Brand Readiness, 10%) 가중평균(0~5)


<br>
<br>

### 3. 채점 방식 (5단계 파이프라인)
1. **입력** : 평가 근거는 실시간 Live URL, 프로젝트 루트의 Code ZIP, 스크린샷 그리고 생성 시간으로만 제한
2. **증거** : 모든 세부 점수는 파일 경로, 특정 화면 섹션 등 최소 2개 이상의 명확한 증거가 있어야만 인정
3. **추론** : 코드, 스크린샷의 일부가 누락되었더라도, 폴더 구조나 import 구문 등 명백한 시그널이 있다면 이를 바탕으로 기능을 추론
4. **분석 :** System Prompt와 스코어링 기준을 탑재한 Gemini 모델이 라우팅 구조 및 상태 흐름 분석, 수식 계산을 스스로 하지 않고, 증거 기반의 원점수(Raw Score) 채점표만 JSON 형태로 생성
5. **반복** : AI의 환각을 막기 위해 동일 조건에서 N회 채점을 진행하며, 최고/최저점을 제외한 평균(10% Trimmed Mean)을 최종 반영


<br>
<br>

<br>

## Appendix: 항목별 상세 평가 기준

### Ⅰ. A. 핵심 성능 표준(Core Performance - Weight 0.60)

|**항목**|**가중치**|**평가 기준 요약**|
|---|---|---|
|**의도 파악 (Context)**|0.15|추상적 프롬프트를 실제 UX 문법으로 구조화하는 기획 역량을 평가함. 숨겨진 예외 사항 스스로 구현 시 상향됨.|
|**이미지 생성 (Multimodal)**|0.10|단순 코드를 넘어 서비스 맥락에 맞는 시각 자산 통합 역량을 평가함. 출처 근거(`manifest.json` 등) 누락 시 최대 3.3점으로 캡을 적용함.|
|**로컬 언어 최적화 (Localization)**|0.15|타겟 언어(한국어 등) 일치 여부를 평가함. 날짜/통화 포맷 및 실제 사용 시 어색함이 없는 현지화 수준을 중시함.|
|**기능 완결성 (Logic)**|0.20|단순 UI를 넘어 실제 데이터 흐름, CRUD, 상태 관리 등 비즈니스 로직 작동 여부를 평가함. **[PDL 적용]**|
|**UI 구조 (StandardLayout)**|0.20|헤더/푸터 구조 준수, 반응형 처리, 접근성(ARIA), 그리드 시스템 등 프론트엔드 표준 완성도를 평가함.|
|**생성 깊이 (E2E Depth)**|0.20|워크플로우의 완결성을 평가함. 목록→상세→수정→완료 등 사용자 여정의 최종(Confirmation) 도달 여부를 확인함.|

#### i. Logic (기능 완결성) 체크리스트

- **L1:** UI 인터랙션(필터, 탭)과 빈 상태(Empty State) 구현 여부를 확인함.
    
- **L2:** L1 + CRUD 기능 작동 여부와 폼 에러 처리 여부를 확인함.
    
- **L3:** L2 + 인증, 결제 프로세스 완결성(과정, 실제 결제가 되는것을 판단하지 않음)과 엣지 케이스(예외 상황) 방어 로직 존재 여부를 확인함.
    

### Ⅱ. B. 개발 및 운영 효율(Developer Experience - Weight 0.30)

|**항목**|**가중치**|**평가 기준 요약**|
|---|---|---|
|**코드 품질 (CodeQuality)**|0.40|컴포넌트 분리, 클린 코드, TypeScript 타입, 가독성/호환성/유지보수 용이성을 평가함. **[PDL 적용]**|
|**생성 효율 (GenEfficiency)**|0.40|난이도 대비 생성 속도(Raw)에 실제 구현 깊이(E2EDepth)를 연동한 보정식을 적용하여 얕고 빠른 편향을 차단함. **[PDL 보정식 적용]**|
|**비용 효율 (CostEfficiency)**|0.12|API 사용량 대비 산출물 품질과 외부 라이브러리 의존성 최소화 여부를 평가함. 무료/Mock 데이터 활용 시 기본 점수를 부여함.|
|**오류 처리 (SelfHealing)**|0.08|에러 발생 시 UI 안내(Toast/Modal) 및 복구 로직(Retry, Error Boundary) 존재 여부를 평가함.|

#### i. Code Quality (코드 품질) 기대치

- **L1:** 컴포넌트 분리와 스타일 시스템 준수 여부를 확인함. 백엔드 로직 부재는 감점 요인이 아님.
    
- **L2:** L1 + 데이터 레이어 분리(Hooks/API) 등을 확인함.
    
- **L3:** L2 + 보안 가드(Auth Guard), 민감 정보 처리, 예외처리 설계 여부를 확인함.
    

#### ii. Gen Efficiency (생성 효율) 평가 기준 및 Depth 연동 보정 로직

단순 속도 우위 모델에 대한 과대평가 및 심층 추론 모델의 역차별을 막기 위해, 시간표 기준 원점수(`RawGenEff`)에 결과물의 구현 깊이를 연동하는 수학적 보정식을 적용함.

**[생성 효율 시간표 및 원점수 기준]**

|**레벨**|**5점 (Perfect)**|**4점 (Good)**|**3점 (Avg)**|**1점 (Fail)**|
|---|---|---|---|---|
|**L1**|2분 미만|2~5분|5~10분|20분 초과|
|**L2**|7분 미만|7~12분|12~20분|35분 초과|
|**L3**|12분 미만|12~20분|20~35분|60분 초과|



**[Depth 연동 보정 계산식]**
LLM 연산 환각을 방지하기 위해 프롬프트 내부 계산을 배제하고, 백엔드 검증 파이프라인에서 다음 공식을 연산하여 최종 점수에 반영함.

$$\text{EffectiveGenEff} = \min\left(5.0, \; \text{RawGenEff} \times \frac{\text{E2EDepth}}{\text{ExpectedDepth}}\right)$$

- **ExpectedDepth 상수:** L1 = 1.0 / L2 = 2.0 / L3 = 4.0
    
- **평가 로직:** 심층 워크플로우를 모두 구현한 모델은 소요 시간이 길어 `RawGenEff`가 다소 낮아도 보정 및 점수가 보호되며, 얕은 화면만 빠르게 생성한 모델은 점수가 대폭 삭감됨.
    

### Ⅲ. C. 브랜드 준비도(Brand Readiness - Weight 0.10)

|**항목**|**가중치**|**평가 기준 요약**|
|---|---|---|
|**브랜드 시스템 (IdentitySystem)**|0.40|Tailwind Config 등을 통한 컬러/폰트 변수 관리 및 디자인 토큰 시스템 구축 여부를 평가함.|
|**UI 통일성 (UIConsistency)**|0.35|공통 컴포넌트(Button, Card) 재사용과 여백/타이포 계층의 일관성을 평가함.|
|**에셋 통일성 (AssetCohesion)**|0.25|이미지/아이콘 스타일의 조화를 평가함. 깨진 이미지 없이 브랜드 분위기에 맞는 에셋 사용 여부를 확인함.|




<br>


## 채점기: Vibench Scoring Judge
![](https://i.imgur.com/yKbvN9w.png)



<br>
<br>

## Vibench 링크

- **Vibench 메인 사이트:** [https://viben.vibe-x.app/](https://viben.vibe-x.app/)
- **대체 사이트:** [https://vibench.vibe-x.app/](https://www.google.com/search?q=https://vibench.vibe-x.app/)
