# Viben 벤치마크 보고서
- 이 문서는 바이브 코더 벤치마크 플랫폼 **Viben** 프로젝트의 종합 분석 보고서입니다. 본 보고서는 실제 구축될 서비스의 스코어링 로직을 기반으로 6개 빌더의 성능을 정량화하여 정리했습니다.
- 본 보고서는 6개 빌더에 대해 동일 프롬프트, 동일 조건, 동일 로직을 가진 검정기를 통한 검정을 통해 결과를 산출했으며, 특정 빌더에 후처리를 통한 어떠한 어드벤티지도 부여하지 않았음을 밝힙니다.  

<br>





## 0. 대시보드 및 결과 보고

본 벤치마크는 동일한 시나리오 프롬프트에 대해 각 플랫폼이 생성한 결과물을 **(1) 코드 구조 분석**과 **(2) Live URL 시각 검증**을 통해 교차 평가한 결과입니다. 검정 시 10회의 반복 수행 후 절삭 평균(상/하한을 제외한 평균)을 적용하여 신뢰도를 확보했습니다.

<br>

## 종합 순위

### 1위 : VibeX 
### 2위 : Bolt
### 3위 : Lovable
### 4위 : Replit
### 5위 : Manus
### 6위 : v0

<br>

### 결과 표



<br>

| **Category**       | **Item**                 | **VibeX** | **Bolt** | **Lovable** | **Replit** | **Manus** | **v0**  |
| ------------------ | ------------------------ | --------- | -------- | ----------- | ---------- | --------- | ------- |
| **A. Core (60%)**  | 의도 파악 (Context)          | 4.5       | **4.9**  | 4.4         | 4.8        | 4.8       | 3.9     |
|                    | 이미지 생성 (Multimodal)      | **4.1**   | 4.0      | 3.6         | 3.9        | 3.8       | 3.4     |
|                    | 로컬 언어 최적화 (Localization) | **4.3**   | 4.0      | 3.5         | 3.1        | 3.0       | 3.0     |
|                    | 기능 완결성 (Logic)           | 4.0       | **4.4**  | 4.0         | 4.0        | 4.0       | 3.9     |
|                    | UI 구조 (Layout)           | 4.0       | 4.0      | 4.0         | **4.1**    | 4.0       | 4.0     |
|                    | 생성 깊이 (Depth)            | 4.0       | **4.5**  | 4.1         | 4.0        | 4.0       | 3.5     |
|                    | **A_w (가중평균)**           | 4.09      | **4.35** | 4.00        | 4.02       | 3.99      | 3.71    |
| **B. Dev (30%)**   | 코드 품질 (Quality)          | 4.0       | 4.0      | 4.1         | 4.2        | **4.5**   | 4.0     |
|                    | 생성 효율 (Efficiency)       | **5.0**   | 4.3      | **5.0**     | 4.0        | 3.4       | **5.0** |
|                    | 비용 효율 (Cost)             | **4.0**   | 3.9      | 3.5         | 3.9        | 3.5       | 3.4     |
|                    | 오류 처리 (Healing)          | 4.0       | 3.5      | 4.0         | 3.9        | **4.5**   | 4.0     |
|                    | **B_w (가중평균)**           | **4.40**  | 4.07     | 4.38        | 4.06       | 3.94      | 4.33    |
| **C. Brand (10%)** | 브랜드 시스템 (Identity)       | 4.0       | 3.9      | 4.2         | 4.1        | **4.5**   | 4.0     |
|                    | UI 일관성 (Consistency)     | 4.9       | 4.1      | 4.6         | 4.8        | **5.0**   | 4.9     |
|                    | 자산 통일감 (Cohesion)        | **4.9**   | 4.1      | 4.8         | 4.6        | 4.8       | 4.6     |
|                    | **C_w (가중평균)**           | 4.54      | 4.02     | 4.49        | 4.47       | **4.75**  | 4.46    |
| **Total**          | **Total Score**            | **84.62** | 84.59    | 83.30       | 81.55      | 80.97     | 79.36   |

<br>

### 데이터 기반 분석 결과

<br>

1. **성능과 효율성의 양강 구도**
    
    - **Bolt**는 복잡한 비즈니스 로직 처리 능력을 나타내는 **Core Performance($A_w$)** 에서 1위(4.35)를 기록함.
        
    - **VibeX**는 생성 효율과 운영 용이성을 나타내는 **Developer Experience($B_w$)** 에서 1위(4.40)를 기록함. 
        

2. **품질-속도 간 트레이드오프(Trade-off) 확인**
    
    - **Manus**와 **Replit**은 **Context(4.8)** 및 **Code Quality(4.2~4.5)** 항목에서 상위권을 기록했으나, **GenEfficiency(3.4~4.0)** 에서 저조했음.
        
    - 고품질 코드 생성을 위한 추론 시간 증가에 기인한 것으로 분석됨.
        

3. **시각 검증을 통한 로컬화 격차**
    
    - VibeX(4.3) 와 Bolt(4.0) 는 로컬 언어 및 표기법(Localization)에서 우수한 점수를 기록하여, 수정 없이 즉시 사용 가능한 수준을 보임.
        
    - 반면 Replit(3.1), Manus(3.0), v0(3.0) 등은 여전히 다국어 렌더링이나 로컬 포맷팅에서 약간의 추가 작업이 필요한 상태로 판단됨.
        
<br>


<br>

### 모델 별 상세 평가

각 모델의 평가는 정량적 점수($A_w, B_w, C_w$)와 이미지 분석 기반의 기술적 검증 결과를 토대로 작성되었습니다.

<br>

#### 🥇 1위. VibeX (84.62점)

- **점수 구성:** $A_w$: 4.09 | $B_w$: **4.40 (전체 1위)** | $C_w$: 4.54
    

- **평가:**
    
    - **이미지 품질:** Multimodal(4.1) 점수가 상위권으로 집계됨. 서비스 맥락에 어울리는 고품질 이미지를 생성하여 시각적 완성도를 높였음.
        
    - **효율성:** GenEfficiency(5.0)를 기록함. 생성 속도와 더불어 높은 비용 효율성(Cost 4.0)을 보임.
        
    - **로컬화:** Localization(4.3)으로 전체 모델 중 가장 높은 점수를 기록함. 로컬 폰트 및 포맷 준수율이 높음.
        
    - **에셋 통일성:** Asset Cohesion(4.9) 점수가 높게 산정됨. 로고 및 이미지 에셋이 서비스 목적에 맞게 일관되게 생성됨.
        

- **보완점:**
    
    - 의도 파악: Context(4.5) 점수는 준수하나 Bolt(4.9) 대비 다소 낮아, 매우 복잡한 기획 의도 구현 시에는 디테일한 프롬프트가 요구됨.

<br>

#### 🥈 2위. Bolt (84.59점)

- **점수 구성:** $A_w$: **4.35 (전체 1위)** | $B_w$: 4.07 | $C_w$: 4.02
    

- **평가:**
    
    - **기능 구현:** Context(4.9) 및 E2E Depth(4.5)에서 높은 수준을 기록함. 복잡한 데이터 플로우와 비즈니스 로직 구현에 비교 모델 중 가장 강력함.
        
    - **로직 안정성:** Logic(4.4) 점수가 높게 평가됨. 백엔드 로직 오류 발생률이 낮은 경향을 보였음.
        

- **보완점:**
    
    - **오류 복구:** SelfHealing(3.5) 점수가 낮게 평가됨. 런타임 에러 발생 시 자동 복구 능력이 상대적으로 부족한 것으로 나타남.
        
    - **디자인 시스템:** Brand Readiness 지표가 4.07로 경쟁사 대비 디자인 일관성 시스템 구축은 다소 약함.

<br>

#### 🥉 3위. Lovable (83.30점)

- **점수 구성:** $A_w$: 4.00 | $B_w$: 4.38 | $C_w$: 4.49
    

- **평가:**
    
    - **밸런스:** 모든 영역($A, B, C$)에서 4.0 이상의 고른 분포를 보임.
        
    - **속도:** GenEfficiency(5.0)를 기록함. 빠른 프로토타이핑에 적합한 성능 특성이 나타남.
        

- **보완점:**
    
    - **멀티모달:** Multimodal(3.6) 점수가 낮게 측정됨. 이미지 에셋의 적절한 배치 및 생성 능력은 상위권 대비 열위로 평가됨.
        
<br>

#### 4위. Replit (83.9점)

- **점수 구성:** $A_w$: 4.02 | $B_w$: 4.08 | $C_w$: 4.47
    

- **평가:**
    
    - **의도 파악:** Context(4.8) 점수가 높게 측정됨. 사용자의 요구사항을 정확하게 구조화하는 능력이 우수한 것으로 확인됨.
        
    - **UI 일관성:** UI Consistency(4.8)를 기록함. 화면 간 디자인 통일성이 높은 편으로 나타나며 Layout(4.1) 점수가 우수하여 안정적인 레이아웃을 제공함.
        

- **보완점:**
    
    - **응답 지연:** 생성 시간이 길어 Efficiency 점수가 낮게 평가됨. 에이전트의 사고 과정(Reasoning)에 리소스를 집중하는 특성에 기인한 것으로 해석됨.
        
    - **로컬화:** Localization(3.1) 점수가 저조함. 한국어 서비스 구축 시 수정 소요가 큰 편으로 판단됨.

<br>

#### 5위. Manus (80.97점)

- **점수 구성:** $A_w$: 3.99 | $B_w$: 3.94 | $C_w$: **4.75 (전체 1위)**
    

- **평가:**
    
    - **품질 우위:** Code Quality(4.5), SelfHealing(4.5), UI Consistency(5.0) 등 산출물의 질적 완성도 측면에서 상위권을 기록함.
        
    - **브랜드 준비도:** 디자인 토큰 및 가이드 준수율이 매우 높은 것으로 평가됨.
        

- **보완점:**
    
    - **효율성 열위:** GenEfficiency(3.4)로 전체 최하위를 기록함. 높은 품질을 위해 생성 시간이 과도하게 소요되어, 신속한 개발이 필요한 환경에는 부적합할 수 있음.

<br>

#### 6위. v0 (79.36점)

- **점수 구성:** $A_w$: 3.71 | $B_w$: 4.33 | $C_w$: 4.46
    

- **평가:**
    
    - **시각화:** GenEfficiency(5.0)와 UI Consistency(4.9)가 높게 측정됨. 프론트엔드 화면 구성 능력은 탁월한 편으로 확인됨.
        

- **보완점:**
    
    - **로직 한계:** Logic(3.9) 및 E2E Depth(3.5) 점수가 낮게 집계됨. 실제 데이터가 연동되는 복잡한 애플리케이션 구현에는 한계가 있는 것으로 평가됨.
        
<br>


<br>

### 종합 결론

본 벤치마크 결과, 6개 플랫폼은 단순한 우열 관계라기보다 각기 다른 **기술적 강점**과 **특화된 영역**을 보유하고 있음이 확인되었습니다. 

<br>

#### ① 엔지니어링 및 고난도 로직 구현

- **Bolt (84.59점)** 는 **Core Performance($A_w$)** 에서 높은 점수를 기록하며, 복잡한 비즈니스 로직과 깊이 있는 워크플로우 등 **백엔드 로직 난이도**가 높은 프로젝트에서 가장 신뢰할 수 있는 성능을 보임.
    

#### ② 심층 추론 및 정교한 코드 설계

- **Manus (80.97점)** 와 **Replit (81.55점)** 은 생성 속도 면에서는 느리지만, 이는 시스템 성능 저하가 아니라 **고품질 산출물을 위한 심층 사고(Reasoning) 과정의 투입**으로 해석됨. 속도보다 **유지보수성이 뛰어난 아키텍처**를 선호하는 환경에 적합.
    

#### ③ UI 프로토타이핑 및 밸런스

- **v0 (79.36점)** 는 프론트엔드 구현 속도와 시각적 품질에 특화되어 있음.
    
- **Lovable (83.30점)** 은 전 영역에서 모난 곳 없는 준수한 성능을 보여, 빠른 아이디어 시각화나 초기 프로토타이핑에 유효한 선택지로 판단됨.
    

#### ④ 비즈니스 실행 및 시장 출시

- **VibeX (84.62점)** 는 **[기획 → 디자인 → 개발 → 마케팅(로고 생성)]의 Full-Cycle 역량**에서 강점을 보임. 이미지 생성의 강점, 브랜드 아이덴티티와 높은 현지화를 통합 지원하여, 개발 직후 즉시 시장 출시가 필요한 비즈니스 프로젝트에 최적화된 솔루션으로 평가됨
    
<br>

<br>


### 아래 부터는 해당 결과가 나오기까지의 채점 과정을 서술합니다.

---


## 1. 평가 도구 소개 및 핵심 평가 원칙

![](https://i.imgur.com/ul3D1Fw.png)


Viben의 채점 시스템은 주관적 해석을 배제하고 객관적 지표를 확보하기 위해 Scoring Judge를 개발하여 채점하며, 평가 시 다음 4가지 원칙을 준수합니다.

1. **입력 데이터 제한**
    
    - 평가 근거는 오직 **(1) Live URL(또는 스크린샷), (2) Code ZIP(프로젝트 루트), (3) Generation Time**만 사용함.
        

2. **증거 기반 채점**
    
    - 모든 세부 항목 점수는 반드시 **2개 이상의 구체적 증거(파일 경로, 특정 화면 섹션 등)** 로 뒷받침되어야 함.
        

3. **합리적 추론**
    
    - 코드 파일이 일부 잘렸더라도 `import` 구문이나 폴더 구조를 통해 기능 존재를 합리적으로 추론할 수 있다면 점수를 인정함.
        

4. **이상치 제어**
    
    - 편차가 큰 항목(Multimodal, Self-healing 등)은 가중치를 낮게 설정함.
        
    - Iteration 모드에서는 Trimmed Mean(최고/최저 제외 평균)을 적용하여 변동성을 최소화함.


        
<br>


## 2. 점수 산정식

총점은 3가지 주요 섹터의 가중 평균을 합산하여 산출됩니다. 정수 단위의 LLM 채점 결과를 정밀한 수학적 모델로 계산합니다.


<br>

### 1) 기본 원칙

- 모든 세부 항목은 **1~5점**으로 평가됩니다.
    

> - **1점 (Critical Failure):** 실행이 불가능하거나, 요구사항과 전혀 다른 결과물로 판정됨.
>     
> - **2점 (Below Standard):** 실행은 되지만 로직이 오작동하거나, UI가 심각하게 깨진 상태로 확인됨.
>     
> - **3점 (Base Standard):** 핵심 기능이 정상 동작하며 에러가 없는 상태로 평가됨.
>     
> - **4점 (Generous Good):** 3점 기준을 충족하며 UX가 매끄럽거나 코드가 깔끔한 경우로 판단됨.
>     
> - **5점 (Strict Perfect):** 단순 구현을 넘어 높은 수준의 디테일이 증명된 경우로 평가됨.
>     
> - **Note:** Evidence(증거)가 없으면 점수는 인정되지 않음. 증거 확보가 불가능한 경우 보수적으로 채점하거나 ‘미확인’ 처리함.
>     


#### Total Score
- 정의: 각 평가 항목의 점수를 가중치에 따라 그대로 합산한 순수 성능 점수.
- 의미: 모델의 절대적인 성능 총점을 나타냄. (1~100점 만점 기준)  
- 계산식: <br>

$$\textbf{Raw Score} = (A_w \times 12) + (B_w \times 6) + (C_w \times 2)$$

- $A_w$: 핵심 성능(Core Performance, 60%) 가중평균(0~5)
    
- $B_w$: 개발 효율(Developer Experience & Efficiency, 30%) 가중평균(0~5)
    
- $C_w$: 브랜드(Brand Readiness, 10%) 가중평균(0~5)

    
<br>
<br>

### 2) 평가 섹터 상세

각 섹터 점수($A_w, B_w, C_w$)는 세부 항목의 중요도에 따른 가중 평균으로 계산됩니다.

#### **A. Core Performance (핵심 성능 - 60%)**

> **목적:** 사용자의 의도를 “작동하는 앱”으로 구현하는 능력을 평가합니다.

$$A_w = 0.12 \text{Context} + 0.05 \text{MultiModal} + 0.10 \text{Local} + 0.28 \text{Logic} + 0.20 \text{Layout} + 0.25 \text{Depth}$$

#### **B. Developer Experience (개발 및 운영 효율 - 30%)**

> **목적:** 생성 속도/운영 효율/비용 대비 산출물 완성도를 평가합니다.

$$B_w = 0.40 \text{Quality} + 0.40 \text{Efficiency} + 0.12 \text{Cost} + 0.08 \text{SelfHealing}$$

#### **C. Brand Readiness (브랜드 준비도 - 10%)**

> **목적:** 일관된 디자인 시스템과 브랜드 아이덴티티를 갖췄는지 평가합니다.

$$C_w = 0.40 \text{Identity} + 0.35 \text{Consistency} + 0.25 \text{Assets}$$

<br>
<br>


## 3. 항목별 상세 평가 기준

모든 항목은 **1~5점 척도**로 평가되며, 1점(Failure), 2점(Below), 3점(Base), 4점(Good), 5점(Perfect)의 앵커(Anchor) 기준을 따릅니다.

<br>

### PDL(Prompt Difficulty Level) 판정 (L1/L2/L3)

채점 시작 전에 사용자의 입력 프롬프트를 아래 기준으로 **L1/L2/L3 중 하나로 분류**합니다. PDL은 **A-4 Logic**, **B-1 CodeQuality**, **B-2 GenEfficiency** 평가 기준에 반영됩니다.

- **L1 — Landing/소개형**
    
    - 키워드 예: introduce, showcase, landing, portfolio, just introduce
        
    - 섹션 구성/내비/탭·필터/제품 상세(모달·섹션) 중심 소개 경험을 기대함. (1depth)
        
<br>

- **L2 — App/CRUD형**
    
    - 키워드 예: dashboard, admin, CRUD, save/edit/delete, database, API, search+filter+sort
        
    - 데이터 생성·수정·삭제, 폼 검증, 상태관리, 에러 처리를 기대함. + 1depth 이상
        
<br>

- **L3 — Service/E2E형**
    
    - 키워드 예: auth/login, role/permission, payment/checkout, booking, subscription, onboarding
        
    - 인증·권한·결제/완료 상태 포함 다단계 워크플로우를 기대함. + 3depth 이상
        

<br>
<br>

### Ⅰ. A. 핵심 성능 표준(Core Performance - Weight 0.60)

|**항목**|**가중치**|**평가 기준 요약**|
|---|---|---|
|**의도 파악 (Context)**|0.12|프롬프트의 핵심 요구사항 구현 여부를 평가함. 숨겨진 요구사항(빈 상태, 예외 처리)까지 스스로 구현 시 5점으로 인정함.|
|**이미지 생성 (Multimodal)**|0.05|이미지 품질 및 톤앤매너 일치도를 평가함. 출처 근거(`manifest.json` 등)가 없으면 최대 3점으로 제한함.|
|**로컬 언어 최적화 (Localization)**|0.10|타겟 언어(Prompt Language) 일치 여부를 평가함. 날짜/통화/주소 포맷의 현지화 및 UX Writing 자연스러움을 포함함.|
|**기능 완결성 (Logic)**|0.28|단순 UI를 넘어 CRUD, 필터, 폼 검증, 상태 관리 등 실질적 비즈니스 로직 작동 여부를 평가함. **[PDL 적용]**|
|**UI 구조 (StandardLayout)**|0.20|헤더/푸터 구조 준수, 반응형 처리, 접근성(ARIA), 그리드 시스템 완성도를 평가함.|
|**생성 깊이 (E2E Depth)**|0.25|단일 페이지를 넘어선 워크플로우 깊이를 평가함. 목록→상세→수정→완료 등 완결된 사용자 여정(Journey) 구현 여부를 포함함.|

#### i. Logic (기능 완결성) 체크리스트

- **L1:** UI 인터랙션(필터, 탭)과 빈 상태(Empty State) 구현 여부를 확인함.
    
- **L2:** L1 + CRUD 기능 작동 여부와 폼 에러 처리 여부를 확인함.
    
- **L3:** L2 + 인증, 결제 프로세스 완결성(과정, 실제 결제가 되는것을 판단하지 않음)과 엣지 케이스(예외 상황) 방어 로직 존재 여부를 확인함.
    

<br>
<br>

### Ⅱ. B. 개발 및 운영 효율(Developer Experience - Weight 0.30)

| **항목**                     | **가중치** | **평가 기준 요약**                                                              |
| -------------------------- | ------- | ------------------------------------------------------------------------- |
| **코드 품질 (CodeQuality)**    | 0.40    | 컴포넌트 분리, 클린 코드, TypeScript 타입 정의, 커스텀 훅 패턴화 등 가독성/호환성/유지보수 용이성을 평가함. **[PDL 적용]**      |
| **생성 효율 (GenEfficiency)**  | 0.40    | 난이도(PDL) 대비 생성 시간 효율성을 평가함. 빌드 실패 시 최대 3점 상한을 적용함.   **[PDL 적용]**                     |
| **비용 효율 (CostEfficiency)** | 0.12    | API 사용량 대비 산출물 품질과 외부 라이브러리 의존성 최소화 여부를 평가함. 무료/Mock 데이터 활용 시 기본 점수를 부여함. |
| **오류 처리 (SelfHealing)**    | 0.08    | 에러 발생 시 UI 안내(Toast/Modal) 및 복구 로직(Retry, Error Boundary) 존재 여부를 평가함.     |
|                            |         |                                                                           |


#### i. Code Quality (코드 품질) 기대치

- **L1:** 컴포넌트 분리와 스타일 시스템 준수 여부를 확인함. 백엔드 로직 부재는 감점 요인이 아님.
    
- **L2:** L1 + 데이터 레이어 분리(Hooks/API) 등을 확인함.
    
- **L3:** L2 + 보안 가드(Auth Guard), 민감 정보 처리, 예외처리 설계 여부를 확인함.
    

<br>

#### ii. Gen Efficiency (생성 효율) 시간표

|**레벨**|**5점 (Perfect)**|**4점 (Good)**|**3점 (Avg)**|**1점 (Fail)**|
|---|---|---|---|---|
|**L1**|2분 미만|2~5분|5~10분|20분 초과|
|**L2**|7분 미만|7~12분|12~20분|35분 초과|
|**L3**|12분 미만|12~20분|20~35분|60분 초과|

<br>
<br>

### Ⅲ. C. 브랜드 준비도(Brand Readiness - Weight 0.10)

|**항목**|**가중치**|**평가 기준 요약**|
|---|---|---|
|**브랜드 시스템 (IdentitySystem)**|0.40|Tailwind Config 등을 통한 컬러/폰트 변수 관리 및 디자인 토큰 시스템 구축 여부를 평가함.|
|**UI 통일성 (UIConsistency)**|0.35|공통 컴포넌트(Button, Card) 재사용과 여백/타이포 계층의 일관성을 평가함.|
|**에셋 통일성 (AssetCohesion)**|0.25|이미지/아이콘 스타일의 조화를 평가함. 깨진 이미지 없이 브랜드 분위기에 맞는 에셋 사용 여부를 확인함.|


<br>
<br>

## 4. 평가 방법

공정성을 보장하기 위해 다음 5단계 파이프라인을 거쳐 최종 점수를 확정합니다.

#### 1. Code Digest
- 프로젝트 ZIP 파일을 분석하여 파일 구조, 주요 코드, 프레임워크 시그널을 요약함.
        
#### 2. Image extraction
- 프로젝트 내, Live URL의 실제 페이지에 접근해 이미지를 추출 

#### 3. Gemini Analysis
- System Prompt와 스코어링 기준을 탑재한 Gemini 모델이 증거를 기반으로 JSON 형태의 채점표를 생성함.
        

#### 4. Validation & Calculation
- 파이썬 스크립트가 JSON을 파싱하여 수학적 정합성을 검증함.
- 가중치 공식을 적용하여 점수를 계산함.
        
#### 5. Iteration
- Temperature 0.4 환경에서 N회 채점을 수행함.
- 극단값(Outlier)을 제거한 뒤 최종 평균을 산출함.

<br>
<br>

## 5. 부록(Appendix)
Test Date: 2026-02-11

<br>


### 사용 프롬프트(1) - APPLE 홈페이지 카피
- PDL L1으로 채점되어 Landing Page only(1 Depth) 기준 채점

```
I want to build a website for apple just introduce about apple products.
```

### 생성 결과(1)
1. lovable - 3m (생성 소요 시간)
	- https://apple-sparkle-hub.lovable.app
2. v0 - 4m 
	- https://v0-apple-product-website-nine.vercel.app/
3. Replit - 3m 
	- https://apple-product-intro.replit.app
4. Bolt - 2m 
	- https://apple-products-websi-l1p1.bolt.host/
5. Manus - 9m 
	- https://appleprod-jpkhapic.manus.space/
6. VibeX - 4m 
	- https://apple-products-showcase-122120.vibe-x.app/

<br>


### 사용 프롬프트(2) - 럭셔리 쇼핑몰 
- PDL L3로 채점되어 인증·권한·결제/완료 상태 등의 다단계 워크플로우 구현 요구(3 Depth 이상)

```
Create a luxury women's fashion e-commerce platform with complete shopping experience: 1. Product Catalog: Display luxury items with high-quality images, price, brand 2. Product Detail: Size selector, color variants, zoom images, add to cart 3. Shopping Cart: View cart, update quantities, remove items, see total 4. Checkout Flow: Shipping info, payment method selection, order summary 5. User Account: Order history, saved addresses, wishlist 6. Elegant UI: Minimalist luxury aesthetic, smooth transitions, premium feel Must include: Product grid, filters, cart drawer, checkout steps, confirmation page.
```

<br>


### 생성 결과(2)
1. lovable - 8m (생성 소요 시간)
- https://luxe-envy-shop.lovable.app
2. v0 - 10m 
- https://v0-luxury-fashion-e-commerce-three.vercel.app/
3. Replit - 15m
- https://chic-boutique.replit.app
4. Bolt - 15m 
- https://luxury-women-s-e-com-1z3j.bolt.host/
5. Manus - 20m
- https://luxefashion-6tnfpvyv.manus.space/
6. VibeX - 9m
- https://maison-elegance-181313.vibe-x.app

<br>
<br>

### Viben Scoring Judge
- https://drive.google.com/drive/folders/1btsCWUayXrqVK8LnxCzyD4d_skYlXc1I?usp=sharing
![](https://i.imgur.com/yKbvN9w.png)

<br>
<br>

### Viben
- https://vibench.base44.app
![](https://i.imgur.com/b3Qx2z1.png)
