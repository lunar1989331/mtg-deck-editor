# MTG 牌組編輯器 — 功能追加規劃文件

- **專案**：`lunar1989331/mtg-deck-editor`（單檔 vanilla JS + Scryfall API，GitHub Pages 部署）
- **文件用途**：記錄待追加功能的規格草稿，供 Claude Code 逐項執行。想到新點子隨時往下加，不急著跑。
- **建議執行順序**：Phase A → B → C →（未來）D
- **最後更新**：2026-07-04

---

## Phase 0（前置，可選）：模組化重構

單檔架構隨功能增加會越來越難維護。建議在動 Phase B 之前，讓 Claude Code 順手把檔案拆成 ES modules（GitHub Pages 原生支援，不需打包工具）。

建議拆分：
- `api.js` — Scryfall 呼叫與快取
- `deck.js` — 牌組資料結構與驗證邏輯
- `ui.js` — DOM 渲染與事件
- `formats.js` — 各賽制規則定義（Phase B 之後會很需要）

驗收標準：功能與現況完全一致，無回歸。

---

## Phase A：備牌（Sideboard）

**難度：低**

### 需求
- 牌組資料結構新增 `sideboard` 陣列（上限 15 張）
- UI 新增備牌區塊，與主牌組並列或分頁顯示
- 支援卡牌在主牌組 ↔ 備牌之間移動（按鈕或拖曳皆可，先做按鈕最省事）
- 匯出／匯入格式需包含備牌（沿用常見的 `SB:` 前綴或空行分隔慣例，與 MTGO / Arena 匯入格式相容）

### 驗收標準
- 備牌超過 15 張時警告
- 匯出的清單能直接貼進 MTG Arena 匯入

---

## Phase B：指揮官（Commander / EDH）支援

**難度：中**

### 需求
- 賽制選擇器：Standard / Modern / Commander（未來可擴充）
- 指揮官指定區：
  - 驗證是傳奇生物，或規則文字含「can be your commander」
  - 支援雙指揮官（Partner）可列為 v2，先不做
- 100 張總數驗證（含指揮官）
- 單卡規則：除基本地外每張限 1 張
- **顏色標識（Color Identity）驗證**：
  - 直接使用 Scryfall 的 `color_identity` 欄位，不要自己解析魔法力符號（規則文字內的符號也算標識，自己解析容易漏）
  - 加入不符合指揮官顏色標識的卡時即時警告

### 驗收標準
- 用一副已知合法的 EDH 牌表匯入，驗證全綠
- 故意加一張顏色標識外的卡，正確跳警告

---

## Phase C：規則式自動組牌（Auto-Build）

**難度：中高／純前端可完成，不需後端**

### 使用者輸入
- 配色（WUBRG 複選）
- 賽制（決定卡池與張數）
- 可選：核心卡或主題關鍵字（例如「Knights」）
- 可選：原型（Archetype）：快攻 Aggro / 控制 Control / 中速 Midrange / 部族 Tribal

### 流程
1. 用 Scryfall 搜尋語法抓候選卡池，例如：
   - `c<=wu f:standard t:creature cmc<=2` （白藍 Standard 低費生物）
   - 可用 `order:edhrec` 或 `order:usd` 排序取熱門卡
2. 套用**原型模板骨架**（先內建 3~4 個模板，這是成品有沒有「靈魂」的關鍵）：
   - 例：60 張 Aggro ≈ 22 地 / 28 生物 / 10 咒語，曲線集中 1–3 費
   - 例：60 張 Control ≈ 26 地 / 8 生物 / 26 咒語（去除、抽牌、終結者各佔比）
3. 按曲線位置與功能角色（去除 / 抽牌 / 終結者 / 節奏）逐格填卡
4. 自動配地：按配色比例分配基本地，v2 再考慮雙色地

### 已知難點（先記著）
- **協同性**：每張卡單獨強 ≠ 整副有配合。靠原型模板 + 主題關鍵字過濾來緩解
- Scryfall 有 rate limit（約 10 req/s），批次搜尋要加延遲
- Commander 的自動組牌規則不同（100 單卡），建議 v1 先只支援 60 張構築

### 驗收標準
- 選「白色 + Standard + Aggro」能產出一副賽制合法、曲線合理的 60 張牌組
- 產出結果可直接進編輯器繼續手動調整

---

## Phase D（未來）：AI 推薦組牌

**前置條件：需要後端 proxy 藏 API key（可與 Hotel English Rescue Phase 2 的 serverless 經驗共用）**

- 讓 LLM 根據自然語言描述（「我想組騎士部族，偏防守」）從卡池選卡
- 或作為 Phase C 的加強層：演算法先組骨架，LLM 負責微調與說明每張卡的入選理由
- 暫緩，等 serverless 基礎建好再回頭

---

## 待辦想法區（隨時追加）

- [ ] （空著等主人想到新點子）
