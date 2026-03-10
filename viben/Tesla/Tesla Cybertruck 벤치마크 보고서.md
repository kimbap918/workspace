## 목표
시중 vibe builder의 성능을 다각도로 비교함과 동시에 3D 구현 성능을 정량평가 하고자 테스트를 실시함. 

<br>

## 1. 테스트 환경


1. 입력 프롬프트
```
인터랙티브하게 회전·확대가 가능한 3D 사이버트럭 모델과 스크롤 기반 애니메이션으로 차량 각도와 주요 스펙이 단계적으로 나타나는 미래지향적 다크 테마의 테슬라 사이버트럭 쇼케이스 랜딩 페이지를 생성해주세요.
```

2. 검정 모델 : gemini-2.5-pro


3. 검정 시행 횟수 : 10회



<br>



## 2. 시행 결과
| **분류**         | **세부 항목**                | **vibex(sonnet2)** | **replit** | **vibex(sonnet1)** | **vibeX(opus)** | **lovable** | **manus** | **v0**    | **bolt**  |
| -------------- | ------------------------ | ------------------ | ---------- | ------------------ | --------------- | ----------- | --------- | --------- | --------- |
| A. Core (60%)  | 의도 파악 (Context)          | 4.6                | 4.5        | 4.6                | 4.6             | 4.4         | 4.5       | 3.7       | 1.7       |
|                | 이미지 생성 (Multimodal)      | 4.5                | 3.9        | 4.6                | 3.8             | 4.2         | 4.5       | 3.4       | 2.4       |
|                | 로컬 언어 최적화 (Localization) | 4.6                | 4.5        | 4.6                | 4.6             | 4.4         | 4.5       | 4.2       | 3.3       |
|                | 기능 완결성 (Logic)           | 4.6                | 4.5        | 4.5                | 4.6             | 3.7         | 4.5       | 3.6       | 1.7       |
|                | UI 구조 (Layout)           | 4.5                | 4.5        | 4.3                | 4.0             | 3.9         | 4.5       | 3.6       | 1.7       |
|                | 생성 깊이 (Depth)            | 4.6                | 4.5        | 4.6                | 4.6             | 4.3         | 4.5       | 4.0       | 1.7       |
|                | **A_w (가중평균)**           | **4.533**          | **4.434**  | **4.490**          | **4.366**       | **4.143**   | **4.530** | **3.767** | **2.009** |
| B. Dev (30%)   | 코드 품질 (Quality)          | 4.2                | 4.3        | 4.0                | 3.8             | 4.4         | 4.5       | 3.7       | 3.6       |
|                | 생성 효율 (Efficiency)       | 5.0                | 5.0        | 5.0                | 5.0             | 5.0         | 2.0       | 5.0       | 1.0       |
|                | 비용 효율 (Cost)             | 3.7                | 4.5        | 4.2                | 3.5             | 3.4         | 3.3       | 3.0       | 4.1       |
|                | 오류 처리 (Healing)          | 4.6                | 4.5        | 4.6                | 4.6             | 3.7         | 4.5       | 3.1       | 2.3       |
|                | **B_w (가중평균)**           | **4.476**          | **4.622**  | **4.459**          | **4.294**       | **4.477**   | **3.369** | **4.078** | 2.538     |
| C. Brand (10%) | 브랜드 시스템 (Identity)       | 4.4                | 4.5        | 4.4                | 3.8             | 4.4         | 4.5       | 3.9       | 3.2       |
|                | UI 일관성 (Consistency)     | 4.6                | 4.5        | 4.6                | 4.4             | 4.4         | 4.5       | 4.4       | 1.8       |
|                | 자산 통일감 (Cohesion)        | 4.6                | 4.5        | 4.6                | 4.6             | 4.4         | 4.5       | 4.4       | 3.0       |
|                | C_w (가중평균)               | 4.484              | 4.490      | 4.490              | 4.181           | 4.440       | 4.530     | 4.192     | 2.670     |
| Total          | **Total Score**          | **90.22**          | **89.92**  | **89.62**          | **86.52**       | **85.46**   | **83.64** | **78.05** | **44.68** |


<br>

#### 1. Lovable  
**Live URL :** [https://cybertruck-dreamscape.lovable.app](https://cybertruck-dreamscape.lovable.app)     <br>
**소요 시간 :** 2m 7s <br>
**Avg Total Score :** 85.46  <br>
**토큰/비용 :** Total In=637,221 | Total Out=24,888 | 총 비용=$1.045406  <br>

<br>

#### 2. V0  
**Live URL :** [https://v0-tesla-cybertruck-showcase.vercel.app/](https://v0-tesla-cybertruck-showcase.vercel.app/)    <br>
**소요 시간 :** 3m 1s   <br>
**Avg Total Score :** 78.05  <br>
**토큰/비용 :** Total In=107,971 | Total Out=25,303 | 총 비용=$0.387994  <br>

<br>

#### 3. Replit  
**Live URL :** [https://cyber-truck-showcase.replit.app](https://cyber-truck-showcase.replit.app)    <br>
**소요 시간 :** 10m  <br>
**Avg Total Score :** 89.92  <br>
**토큰/비용 :** Total In=842,304 | Total Out=30,220 | 총 비용=$1.355080  <br>

<br>

#### 4. Manus  
**Live URL :** [https://cybertruck-hozb4wjh.manus.space/](https://cybertruck-hozb4wjh.manus.space/)   <br>
소요시간 : 19m 15s  <br>
**Avg Total Score :** 83.64  <br>
**토큰/비용 :** Total In=1,088,181 | Total Out=29,204 | 총 비용=$1.652266  <br>

<br>

#### 5. Bolt
**Live URL :** https://3d-cybertruck-showca-sgel.bolt.host/  <br>
**소요 시간 :** 30m over  <br>
**Avg Total Score :** 44.68  <br>
**토큰/비용 :** Total In=95,781 | Total Out=22,987 | 총 비용=$0.349596  <br>

<br>

#### 6. VibeX(sonnet2)
**Live URL :** [https://cybertruck-showcase-190909.govibex.net](https://cybertruck-showcase-190909.govibex.net)   <br>
**소요 시간 :** 10m 8s   <br>
**Avg Total Score :** 90.22  <br>
**토큰/비용 :** Total In=1,026,807 | Total Out=31,233 | 총 비용=$1.595839  <br>


<br>

#### 7. VibeX(sonnet1)
**Live URL :** [https://cybertruck-showcase-183308.govibex.net](https://cybertruck-showcase-183308.govibex.net)   <br>
**소요 시간 :** 9m7s  <br>
**Avg Total Score :** 89.62  <br>
**토큰/비용 :** Total In=1,061,479 | Total Out=31,077 | 총 비용=$1.637619  <br>


<br>

#### 8. Vibex(Opus) 
**Live URL :** https://cybertruck-showcase-094734.govibex.net](https://cybertruck-showcase-094734.govibex.net  <br>
**소요 시간 :** 10m22s  <br>
**Avg Total Score :** 86.52  <br>
**토큰/비용 :** Total In=907,301 | Total Out=29,014 | 총 비용=$1.424266  <br>


<br>


## 3. 토큰/비용 결과
**전체 Total In (입력 토큰):** 5,767,045  <br>
**전체 Total Out (출력 토큰):** 223,926  <br>
**전체 총 비용:** $9.448066  <br> 
- 2026년 3월 10일 기준, 1달러 = 약 1,468.48원을 적용했을 때, 한화로 **약 13,874원 발생**



(합산 참고)
- **Lovable:** In=637,221 / Out=24,888 / Cost=$1.045406
- **V0:** In=107,971 / Out=25,303 / Cost=$0.387994
- **Replit:** In=842,304 / Out=30,220 / Cost=$1.355080
- **Manus:** In=1,088,181 / Out=29,204 / Cost=$1.652266
- **Bolt:** In=95,781 / Out=22,987 / Cost=$0.349596
- **VibeX(sonnet2):** In=1,026,807 / Out=31,233 / Cost=$1.595839
- **VibeX(sonnet1):** In=1,061,479 / Out=31,077 / Cost=$1.637619
- **Vibex(Opus):** In=907,301 / Out=29,014 / Cost=$1.424266
