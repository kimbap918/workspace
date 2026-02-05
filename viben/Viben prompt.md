## 0) Project / Product

- Product: **Viben**
- Slogan: **The Vibe Standard**
- Goal: Quantify End-to-End vibe-coding tools (0–100) and provide a public leaderboard; compare scenario results (One-shot vs Iterative).



## 1) Visual Identity & Design Style

### Theme
- **Neo-Brutalism + Glassmorphism** high-tech style

### Color
- Background: **#1A1A1B**
- Highlights: Electric Blue + Neon Green (accent only; readability first)
- Neutral grays for text/borders/sub-tones

### Layout
- Bento Grid: dense summary cards (Top 3, IPI, badges, recent runs)
- Leaderboard: full-width table, sticky header, horizontal scroll, column toggle/sort/filter
- Enough negative space

### Interaction
- Route/section fade-in transitions
- Score counting animation (with Reduce Motion)
- Tool cards: strong 3D hover only for Top3; subtle for others
- Glass blur moderate (8–12px), layered feel via noise/grid

### Accessibility / Performance (must)
- Reduce Motion toggle
- Contrast/readability priority
- Table/list performance optimization (avoid heavy blur/3D everywhere)



## 2) Internationalization (NEW, must)

**The site’s default language is English.** It must support multiple languages via i18n.

### 2-1) i18n Requirements
- Use a standard i18n library (e.g., **react-i18next**) with:
  - Key-based translations
  - Lazy-loaded locale JSON (code-splitting)
- Default locale: **en**
- Provide at least:
  - **en** (complete)
  - **ko** (complete)
  - Additional locales can be added easily later (e.g., ja, zh, es)
- Add a **Language Switcher** in the top navbar (dropdown).
- Persist language choice:
  - localStorage (preferred) + URL query optional (`?lang=ko`)
- Prepare for RTL expansion later (ar/ar-SA):
  - Use logical CSS where possible and avoid hard-coded left/right assumptions.

### 2-2) Locale Formatting (must)
- Use `Intl` for:
  - Dates (`createdAt`, `runAt`)
  - Numbers (scores)
  - Currency (`usdCost`)
- Localize units (sec, tokens, USD) and ensure readable formatting.

### 2-3) What must be translated
- All UI strings including:
  - Navigation, filters, table headers
  - Score explanations (math / weights)
  - Badges, tool tags, community UI
  - Empty states and tooltips
- Data content:
  - Benchmarks `name`, `category`, and `promptText` should support i18n:
    - Store as `i18n` objects (e.g., `name_i18n: { en, ko }`)
    - English default shown when translation missing.



## 3) IA: Pages & Features (SPA + React Router)

### 3-1) Home (NO marketing hero; visualization hero)
- Hero = 3 charts (top row) visible immediately:
  1. **IPI Index Top10 Bar Chart**
  2. **Speed (generation efficiency) Bar Chart**
  3. **Cost (cost efficiency) Bar Chart**
- “The Vibe Standard” should be a small subtitle only.
- IPI toggle:
  - IPI = (One-shot Total + Iterative Total) / 2 (default)
  - Toggle between One-shot / Iterative / IPI
- Bento summary cards:
  - Top 3 tools (strong 3D hover)
  - Best badges
  - Recent runs (5)
  - Quick access to 3 benchmarks

### 3-2) Benchmarks
- 3 scenario cards:
  - PSP guide app
  - Luxury women’s fashion shopping mall (detail/cart/checkout complete)
  - Instagram-like feed
- Scenario detail:
  - Prompt text + copy
  - One-shot / Iterative tabs
  - Side-by-side viewer:
    - Tool A vs Tool B
    - previewUrl (iframe or external) + screenshot (optional)
    - Preset buttons: **VibeX vs Replit**
  - **Generative Image vs Stock Search contrast UI (must)**
    - In VibeX/Replit preset: overlay labels `Matched` / `Mismatch`
    - Badges `Generative Image` / `Stock Search`
    - Works with dummy data; auto overlay if “image verdict” exists

### 3-3) Leaderboard (Results)
- Full table (0–100)
- Filters: runType, benchmark, modelName, category
- Sorting: total, A/B/C, each sub-metric
- Columns:
  - Rank, Tool, Total, A.Core(60), B.DX(30), C.Branding(10), Badges, Detail
- Column toggle, sticky header, responsive (horizontal scroll)

### 3-4) Tool Detail
- One-shot / Iterative tabs
- Radar chart for A/B/C
- Bars below for exact sub-metrics
- VibeX only: Branding Workflow 7-step infographic
- Badge click => Evidence modal (auto-generated from scoring)

### 3-5) Admin Panel
- No auth initially, but extensible
- CRUD by **Run**:
  - toolId, benchmarkId, runType, modelName, previewUrl, notes
  - Fact fields (store numeric):
    - generationTimeSec
    - tokenInput, tokenOutput, usdCost
    - implementedDepth
  - Ratings (0–5, step 0.5):
    - A Core:
      - context, multimodal, localization, logic, standardLayout, mobileOptimization,
      - e2eDepthCompleteness (NEW)
    - B DX:
      - codeQuality, advancedFeatures, physicalCompleteness,
      - generationEfficiency (NEW), costEfficiency (NEW)
    - C Branding:
      - brandingEngine
- Auto-calc A/B/C/Total on change
- Edit/delete + simple version history

### 3-6) Community Board (NEW)
- Users can propose scores and discuss
- `/community` lists threads by run (filterable)
- Each thread:
  - preview link/screenshot
  - comments + tags
  - propose ratings (0–5, 0.5) + rationale
- Show **Official vs Community consensus** toggle on top
- `/community/thread/:id` detailed view with history
- Data structure extensible for verified/moderation later



## 4) Scoring Logic (show in UI)

### 4-1) Display math
To quantify vibe-coding tool performance, apply a weighted formula. All metrics are rated on a **0–5** scale.

$$Total Score = (A_{avg} \times 12) + (B_{avg} \times 6) + (C_{score} \times 2)$$

**Metric details ($W$ = Weight):**

- **A. Core Performance (60%):** How accurately the tool implements the user’s intent as a working app.
  - $A_{avg} = \frac{\sum(\text{Context} + \text{Multimodal} + \text{Localization} + \text{Logic} + \text{Standard Layout} + \text{E2E Depth})}{6}$

- **B. Developer Experience & Efficiency (30%):** Speed, code completeness, and cost efficiency.
  - $B_{avg} = \frac{\sum(\text{Code Quality} + \text{Gen Efficiency} + \text{Cost Efficiency} + \text{Self-healing})}{4}$

- **C. Branding Engine (10%):** Ability to establish brand identity (logo, font, color guide).
  - $C_{score} = \text{Branding Engine Workflow Score}$

### 4-2) One-shot vs Iterative standard
- One-shot: first prompt only
- Iterative: max 3 revisions (store history)
- Iterative changes are for bug-fix/requirements; feature adds allowed but logged


| **분류**       | **세부 항목**           | **VibeX** | **Lovable** | **Mocha** | **Bolt**  | **Ready** | **v0**    | **Tempo** | **Manus** | **Poly**  | **Replit** |
| ------------ | ------------------- | --------- | ----------- | --------- | --------- | --------- | --------- | --------- | --------- | --------- | ---------- |
| **A. Core**  | 의도 파악 (Context)     | **4.5**   | **4.5**     | **4.5**   | 4.0       | 3.5       | 4.0       | 4.2       | **4.5**   | 3.8       | 3.5        |
| **(60%)**    | 이미지 생성 (Multimodal) | **4.5**   | 3.5         | 2.5       | 3.0       | **4.5**   | 3.0       | 2.5       | 2.0       | 3.5       | 2.5        |
|              | 로컬 언어 최적화           | **3.5**   | 3.0         | 3.0       | 2.5       | 3.0       | 3.0       | 2.5       | 3.0       | 2.5       | 2.5        |
|              | 기능 완결성 (Logic)      | **5.0**   | 4.7         | **5.0**   | 4.8       | 3.5       | 4.5       | 4.0       | 4.5       | 3.8       | **5.0**    |
|              | UI 구조 (Layout)      | **5.0**   | 4.5         | 4.0       | 4.5       | 4.0       | **5.0**   | 4.5       | 4.0       | 4.5       | 4.0        |
|              | 생성 깊이 (Depth)       | **5.0**   | 4.5         | 4.5       | 4.0       | 4.0       | 2.5       | 4.0       | 4.2       | 2.5       | **5.0**    |
|              | **Aavg (평균)**       | **4.58**  | **4.12**    | **3.92**  | **3.80**  | **3.75**  | **3.67**  | **3.62**  | **3.70**  | **3.43**  | **3.75**   |
| **B. Dev**   | 코드 품질 (Quality)     | 4.5       | 4.8         | 4.5       | **5.0**   | 4.0       | 4.8       | 4.5       | 4.2       | **5.0**   | 4.2        |
| **(30%)**    | 생성 효율 (Efficiency)  | **4.8**   | **4.8**     | 4.0       | 4.5       | 4.5       | **4.8**   | 4.2       | 3.5       | 4.0       | 2.5        |
|              | 비용 효율 (Cost)        | 4.5       | 4.0         | 4.5       | 4.0       | 4.5       | 4.5       | 4.0       | 3.0       | 4.0       | 2.5        |
|              | 오류 처리 (Healing)     | 4.2       | 4.5         | 4.3       | 4.7       | 3.0       | 4.0       | 4.0       | 3.5       | 3.5       | **5.0**    |
|              | **Bavg (평균)**       | **4.50**  | **4.53**    | **4.33**  | **4.55**  | **4.00**  | **4.53**  | **4.18**  | **3.55**  | **4.13**  | **3.55**   |
| **C. Brand** | 브랜딩 엔진 (Workflow)   | **4.5**   | 4.0         | 3.0       | 2.5       | 4.0       | 2.5       | 3.5       | 4.0       | 3.5       | 2.0        |
| **Total**    | **종합 점수**           | **91.00** | **84.62**   | **78.98** | **77.90** | **77.00** | **76.22** | **75.44** | **73.70** | **71.86** | **70.30**  |


## I. A. Core Performance Standard

Measures accuracy and depth in converting user requirements into working software.

### 1. Context Understanding
- **Definition:** Ability to infer business logic, brand story, and unstated required features from a short/abstract prompt.
- **Scoring Basis:** Higher scores when the tool proactively plans and implements key user flows and adds context-appropriate elements beyond simple text transformation.

### 2. Multimodal Generation
- **Definition:** Ability to generate or place high-resolution visuals aligned with content context and product naming.
- **Scoring Basis:** Higher scores for AI-generated visuals that precisely reflect prompt attributes (color, item type, model wearing, etc.) rather than stock search matching (higher mismatch risk).

### 3. Localization
- **Definition:** Understanding subtle local nuances and handling local data (names, places, tone) suitable for the local market.
- **Scoring Basis:** Penalize English-only dummy data; reward natural local nicknames/comments/addresses and appropriate local typography.

### 4. Logic Implementation
- **Definition:** Degree to which the UI includes real, working logic (data handling, navigation, test auto-login, etc.).
- **Scoring Basis:** Higher scores for interactive MVP-level execution versus static mockups.

### 5. Standard Layout & UX
- **Definition:** Compliance with standard mobile/web UI patterns (tab bar, nav bar, spacing) and typography polish.
- **Scoring Basis:** Evaluate alignment with platform guidelines (Material Design, HIG) and quality of CSS styling and responsive layout.

### 6. E2E Depth
- **Definition:** Ability to build the full service workflow in one go (main → detail → cart → checkout → confirmation), beyond a single landing page.
- **Scoring Basis:** Measure number of pages generated per query, logical linkage, and completeness without missing business steps.



## II. B. Developer Experience & Operations Efficiency

Measures productivity, code quality, and post-generation maintenance capability.

### 7. Code Quality
- **Definition:** Readability, adherence to modern framework conventions, and maintainability.
- **Scoring Basis:** Evaluate folder structure, duplication, styling cleanliness (CSS/Tailwind), and compatibility with common dev environments.

### 8. Generation Efficiency
- **Definition:** Time-to-result and whether planning/steps are visible during generation.
- **Scoring Basis:** Higher scores for faster completion and clear step-by-step progress visibility (file creation, package install, etc.).

### 9. Cost Efficiency
- **Definition:** Feature coverage vs subscription cost, API usage efficiency, and included services (deploy/hosting).
- **Scoring Basis:** Consider monthly fee vs project output, agent performance, and infra cost savings.

### 10. Self-healing
- **Definition:** Ability to analyze runtime error logs and self-correct, or recover broken UI via user feedback.
- **Scoring Basis:** Reward automatic terminal log analysis and “Fix it”-style flows, proactive diagnosis panels, and reliable redeploy.
