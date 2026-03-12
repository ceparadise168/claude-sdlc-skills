# Claude SDLC Skills

**讓 AI 幫你把關每一個開發環節 — 從需求到上線，不再遺漏關鍵步驟。**

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
| 架構討論只存在口頭或白板，事後沒人記得 | 用 ASCII 圖確認架構，產出 Design Doc 留存 |
| 上線前才發現沒想到的 edge case | 系統性檢查 11 類邊界案例（併發、注入攻擊、認證錯誤...） |
| 壓力測試被跳過，上線後才爆炸 | 強制要求 load / spike / soak / capacity 四種壓測 |
| Release 出問題不知道怎麼 rollback | 提前產出 rollback 計畫，含觸發條件和具體步驟 |
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
        │  需求定義     │  ← user stories、驗收標準、PRD
        └──────┬──────┘
          Gate 1 ✓ 你確認
        ┌──────v──────┐
        │  架構設計     │  ← ASCII 圖、API 合約、領域模型
        └──────┬──────┘
          Gate 2 ✓ 你確認
        ┌──────v──────┐
        │  測試規劃     │  ← 邊界案例、壓測計畫、E2E 場景
        └──────┬──────┘
          Gate 3 ✓ 你確認
        ┌──────v──────┐
        │  上線準備     │  ← release checklist、rollback 計畫
        └──────┬──────┘
          Gate 4 ✓ 你確認
               │
               v
           上線部署
```

每個 Gate 都會暫停，列出待確認事項和風險，等你說「OK」才繼續。

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

你：confirmed

AI：進入測試規劃...
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

- **User Stories** — 符合 INVEST 原則（Independent, Negotiable, Valuable, Estimable, Small, Testable）
- **驗收標準** — Given/When/Then 格式，可直接轉化為測試案例
- **PRD 模板** — 問題陳述、商業目標、範圍界定、利害關係人、成功指標
- **需求分級** — Must / Should / Could 優先級
</details>

<details>
<summary><b>架構設計</b> — 用圖說話，確認後再動手</summary>

- **ASCII 圖確認協議** — 先畫圖 → 標記不確定處 → 你確認 → 才鎖定設計
- **Domain-Driven Design** — 識別 Bounded Context、Aggregate、Entity、Value Object
- **Clean Architecture** — 領域層 → 應用層 → 介面層 → 基礎設施層，依賴方向向內
- **Design by Contract** — 每個 API 定義前置條件、後置條件、不變量
- **12-Factor App** — 組態分離、無狀態程序、日誌串流、快速啟停
</details>

<details>
<summary><b>測試規劃</b> — 不只測 happy path</summary>

- **11 類邊界案例** — 併發/競態、大數值/零值/負值、SQL 注入/XSS、401/403 認證錯誤、資料庫鎖死、冪等性、網路失敗...
- **4 種壓力測試** — Load（基準效能）、Spike（突發流量）、Soak（記憶體洩漏偵測）、Capacity（找到系統極限）
- **Playwright E2E** — 有 UI 就必須有端對端測試
- **韌性測試** — Circuit Breaker、重試機制、級聯失敗
- **測試追溯矩陣** — 每個 user story 對應哪些測試案例
</details>

<details>
<summary><b>上線準備</b> — 不是 deploy 了就結束</summary>

- **Release Checklist** — 程式碼、測試、文件、安全、基礎設施逐項確認
- **Rollback 計畫** — 觸發條件（錯誤率 > X%、延遲 > Y ms）、回滾步驟、驗證方式
- **部署策略建議** — Feature Flag / Canary / Blue-Green / 分階段 / Pilot / Big Bang
- **上線後監控計畫** — 前 24-48 小時該盯哪些指標
- **正式簽核表** — 適用於需要正式審批的 Waterfall 流程
</details>

---

## 方法論比較

| | Agile | Waterfall | Iterative |
|--|-------|-----------|-----------|
| **適合場景** | 需求會變、頻繁發布 | 法規遵循、正式審批 | 工程導向、分階段交付 |
| **文件量** | 輕量 | 完整 | 中等 |
| **審查關卡** | 口頭確認即可 | 正式簽核 | 書面確認 |
| **User Stories** | 聚焦當前 sprint | 全量定義 | 當前迭代 + 整體願景 |

---

## 技能原始碼

想客製化？原始碼在這些目錄裡，歡迎 fork 修改：

```
sdlc-orchestrator/           ← 協調器：方法論選擇、關卡管理、階段排序
sdlc-requirements-shaper/    ← 需求：user stories、驗收標準、PRD 模板
sdlc-architecture-designer/  ← 架構：ASCII 圖、DDD、DbC、設計文件模板
sdlc-test-planner/           ← 測試：邊界案例清單、壓測模式、E2E 模板
sdlc-release-readiness/      ← 上線：release checklist、rollback 計畫模板
```

每個目錄包含 `SKILL.md`（技能定義）和 `references/`（模板和參考文件）。

---

## License

MIT
