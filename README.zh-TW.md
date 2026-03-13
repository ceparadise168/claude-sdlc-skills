# Claude SDLC Skills

> **[English](README.md)** | 正體中文

**讓 AI 幫你把關每一個開發環節 — 從需求到上線，不再遺漏關鍵步驟。**

---

## 設計理念

多數 SDLC 工具把最佳實踐當作規則。這組技能把它們當作**可以選用的工具** — 有意識地、根據當下情境來選擇。

同樣的設計，在不同的時空背景下，會有不同的最適選擇。決定因素可能是團隊經驗、基礎設施成熟度、商業目標、上線時程、利害關係人期待 — 重點不是你有沒有套用 DDD 或 Clean Architecture，而是這個選擇是不是**有意識的**。

**有框架，但不教條。**每個階段提供結構，但深度會根據情境調整。5 人使用的內部工具和百萬用戶的支付系統，經過相同的關卡，但嚴謹度天差地別。

**有意識的取捨，而非無意識的省略。**當建議推遲某些工作 — 跳過完整 PRD、簡化架構、縮減測試範圍 — 會明確點名取捨：換到了什麼、放棄了什麼、什麼條件改變時需要重新評估。有意識的技術債是智慧，沒想清楚就省略只是債務。

**避免過早抽象，但要去思考。**現在不做抽象，不代表不去想。而是你已經思考過，判斷在這個階段做抽象弊大於利。技能會記錄考慮過什麼、為什麼推遲 — 情境改變時可以重新檢視。

**程式碼是流動的。**它服務於會演化的專案概念。這組技能為流動性而設計：結構可以改變形狀而不需要昂貴的重寫，但也不為可能永遠不來的未來過度設計。

**區分原則性的和可調整的。**有些事不可妥協（可測試的驗收標準、正式環境的回滾計畫）。有些隨情境彈性調整（要不要用 DDD、架構分幾層、簽核多正式）。技能會標明哪些是哪些。

---

## 這是什麼？

一組安裝在 [Claude Code](https://claude.com/claude-code) 上的技能包，讓 AI 在軟體開發的每個階段提供結構化的引導：

| 階段 | 你說的話 | AI 幫你做的事 |
|------|---------|--------------|
| 需求定義 | 「幫我寫 checkout 的 user stories」 | 產出符合 INVEST 原則的用戶故事、驗收標準、PRD |
| 架構設計 | 「設計這個服務的架構」 | 用 ASCII 圖確認架構、定義 API 合約、識別領域邊界 |
| 測試規劃 | 「上線前該測什麼？」 | 列出邊界案例、壓力測試計畫、E2E 場景、安全測試 |
| 上線準備 | 「我們準備好上線了嗎？」 | 產出 release checklist、rollback 計畫、go/no-go 評估 |

也可以一次跑完整個流程：

> 「用 Agile 方法，引導這個功能從構想到上線」

AI 會自動切換階段、在每個關卡暫停等你確認，確保沒有環節被跳過。

---

## 為什麼需要這個？

### 解決的痛點

| 常見問題 | 這組技能怎麼幫 |
|---------|--------------|
| 需求不清就開始寫 code，做完才發現方向錯了 | 強制先產出 user stories 和驗收標準，確認範圍再動手 |
| 架構討論只存在口頭或白板，事後沒人記得 | 用 ASCII 圖確認架構，產出設計文件留存 |
| 上線前才發現沒想到的邊界案例 | 系統性檢查 11 類邊界案例（併發、注入攻擊、認證錯誤⋯） |
| 壓力測試被跳過，上線後才出事 | 強制要求 Load / Spike / Soak / Capacity 四種壓測 |
| 出問題不知道怎麼 rollback | 提前產出回滾計畫，含觸發條件和具體步驟 |
| 不同團隊用不同流程，品質參差不齊 | 提供 Agile / Waterfall / Iterative 三種方法論模板，統一基準 |

### 帶來的價值

- **降低返工成本** — 在需求階段就抓出模糊地帶，不是做完才改
- **提升交付品質** — 每個功能都經過相同的品質關卡檢查
- **加速新人上手** — 資淺工程師也能產出資深水準的技術文件
- **建立團隊共同語言** — 需求怎麼寫、架構怎麼畫、測試怎麼規劃，全隊一致
- **知識不再只在少數人腦中** — 流程固化為可重複使用的模板

---

## 運作方式

```
你描述一個功能
        │
        v
┌─────────────────────────────────┐
│  選擇開發方法論                    │
│  Agile / Waterfall / Iterative   │
└──────────────┬──────────────────┘
               │
        ┌──────v──────┐
        │  需求定義     │  ← 用戶故事、驗收標準、PRD
        └──────┬──────┘
          關卡 1 ✓ 你確認
        ┌──────v──────┐
        │  架構設計     │  ← ASCII 圖、API 合約、領域模型
        └──────┬──────┘
          關卡 2 ✓ 你確認
        ┌──────v──────┐
        │  測試規劃     │  ← 邊界案例、壓測計畫、E2E 場景
        └──────┬──────┘
          關卡 3 ✓ 你確認
        ┌──────v──────┐
        │  上線準備     │  ← 上線清單、回滾計畫
        └──────┬──────┘
          關卡 4 ✓ 你確認
               │
               v
           上線部署
```

每個關卡都會暫停，列出待確認事項和風險，等你說「OK」才繼續。

---

## 安裝方式

### 前置條件

- 已安裝 [Claude Code](https://claude.com/claude-code) CLI

### 一鍵安裝（推薦）

```bash
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-orchestrator.skill
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-requirements-shaper.skill
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-architecture-designer.skill
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-test-planner.skill
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-release-readiness.skill
```

### 本地安裝

```bash
git clone https://github.com/ceparadise168/claude-sdlc-skills.git
cd claude-sdlc-skills
claude install-skill sdlc-orchestrator.skill
claude install-skill sdlc-requirements-shaper.skill
claude install-skill sdlc-architecture-designer.skill
claude install-skill sdlc-test-planner.skill
claude install-skill sdlc-release-readiness.skill
```

---

## 使用範例

### 完整流程

```
你：用 Agile 方法，引導「會員點數兌換功能」從需求到上線

AI：好的，我先幫你做需求定義。
    [產出 user stories、驗收標準]
    以上需求確認嗎？

你：確認

AI：進入架構設計階段。
    [產出架構圖、API 合約、領域模型]
    架構確認嗎？

你：確認

AI：進入測試規劃⋯
    （持續到上線準備完成）
```

### 單獨使用某個階段

| 場景 | 你可以這樣說 |
|------|------------|
| 只需要需求文件 | 「幫我寫 billing 功能的 PRD」 |
| 只需要架構設計 | 「設計 notification service 的架構，用 ASCII 圖確認」 |
| 只需要測試計畫 | 「Black Friday 流量暴增 10 倍，幫我規劃壓力測試」 |
| 只需要上線清單 | 「週五要部署新版支付功能，幫我做 release checklist」 |

---

## 涵蓋的工程實踐

<details>
<summary><b>需求定義</b> — 把模糊的想法變成可執行的規格</summary>

- **用戶故事（User Stories）** — 符合 INVEST 原則（獨立、可協商、有價值、可估算、小、可測試）
- **驗收標準** — Given/When/Then 格式，可直接轉化為測試案例
- **PRD 模板** — 問題陳述、商業目標、範圍界定、利害關係人、成功指標
- **需求分級** — Must（必須）/ Should（應該）/ Could（可以）優先級
</details>

<details>
<summary><b>架構設計</b> — 用圖說話，確認後再動手</summary>

- **ASCII 圖確認協議** — 先畫圖 → 標記不確定處 → 你確認 → 才鎖定設計
- **領域驅動設計（DDD）** — 識別限界上下文、聚合根、實體、值物件
- **清晰架構（Clean Architecture）** — 領域層 → 應用層 → 介面層 → 基礎設施層，依賴方向向內
- **契約式設計（Design by Contract）** — 每個 API 定義前置條件、後置條件、不變量
- **十二要素應用（12-Factor App）** — 組態分離、無狀態程序、日誌串流、快速啟停
</details>

<details>
<summary><b>測試規劃</b> — 不只測正常路徑</summary>

- **11 類邊界案例** — 併發/競態、大數值/零值/負值、SQL 注入/XSS、401/403 認證錯誤、資料庫鎖死、冪等性、網路失敗⋯
- **4 種壓力測試** — Load（基準效能）、Spike（突發流量）、Soak（記憶體洩漏偵測）、Capacity（找到系統極限）
- **端對端測試（Playwright E2E）** — 有 UI 就必須有端對端測試
- **韌性測試** — 斷路器（Circuit Breaker）、重試機制、級聯失敗
- **測試追溯矩陣** — 每個用戶故事對應哪些測試案例
</details>

<details>
<summary><b>上線準備</b> — 不是部署了就結束</summary>

- **上線清單（Release Checklist）** — 程式碼、測試、文件、安全、基礎設施逐項確認
- **回滾計畫（Rollback Plan）** — 觸發條件（錯誤率 > X%、延遲 > Y ms）、回滾步驟、驗證方式
- **部署策略建議** — 功能開關（Feature Flag）/ 金絲雀部署（Canary）/ 藍綠部署（Blue-Green）/ 分階段上線 / 試點（Pilot）
- **上線後監控計畫** — 前 24-48 小時該盯哪些指標
- **正式簽核表** — 適用於需要正式審批的瀑布式流程
</details>

---

## 方法論比較

| | 敏捷（Agile） | 瀑布式（Waterfall） | 迭代式（Iterative） |
|--|-------|-----------|-----------|
| **適合場景** | 需求會變、頻繁發布 | 法規遵循、正式審批 | 工程導向、分階段交付 |
| **文件量** | 輕量 | 完整 | 中等 |
| **審查關卡** | 口頭確認即可 | 正式簽核 | 書面確認 |
| **用戶故事** | 聚焦當前迭代 | 全量定義 | 當前迭代 + 整體願景 |

---

## 技能原始碼

想客製化？原始碼在這些目錄裡，歡迎 fork 修改：

```
sdlc-orchestrator/           ← 協調器：方法論選擇、關卡管理、階段排序
sdlc-requirements-shaper/    ← 需求：用戶故事、驗收標準、PRD 模板
sdlc-architecture-designer/  ← 架構：ASCII 圖、DDD、契約式設計、設計文件模板
sdlc-test-planner/           ← 測試：邊界案例清單、壓測模式、E2E 模板
sdlc-release-readiness/      ← 上線：上線清單、回滾計畫模板
```

每個目錄包含 `SKILL.md`（技能定義）、`references/`（模板和參考文件）和 `evals/`（觸發評測測試案例，用於描述優化）。

---

## 授權條款

MIT
