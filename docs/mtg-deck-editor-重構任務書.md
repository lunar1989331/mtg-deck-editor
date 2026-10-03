# MTG Deck Editor 重構任務書

> 供 Claude Fable 5 執行用。請先完整閱讀整份 `mtg-deck-editor.html` 再開始動工。

## 專案背景

- Repo：`lunar1989331/mtg-deck-editor`（GitHub Pages 部署，MIT License）
- 形式:單一 HTML 檔（約 70KB），vanilla JS + Chart.js + Scryfall API + localStorage
- 語言：介面為繁體中文環境（`lang="zh-TW"`），卡片資料為英文
- 設計原則：**維持單檔、零建置、零框架**。不引入 npm、bundler 或前端框架。

## 一、必修 Bug（最優先）

### 1-1. 匯入時張數錯位
`importDeck()` 以「索引位置」將 `chunk.qtys[i]` 對應到 API 回傳的卡片。但 Scryfall `/cards/collection` 遇到查無此卡時會將其移入 `not_found`，回傳的 `data` 陣列會變短，導致後續所有卡片的張數整批錯位。

**修法要求**：改用「卡名比對」而非索引比對（注意雙面牌名稱含 ` // ` 的情形）。

### 1-2. 匯入失敗靜默消失
查無的卡片目前不會通知使用者。請蒐集 `not_found` 清單，匯入完成後以 toast 或彈窗列出「以下 N 張卡片未找到」。

## 二、存檔機制瘦身

`saveDeck()` 目前將完整 Scryfall 卡片物件（含圖片網址、法遵、價格等）整包存入 localStorage，單副套牌可達數百 KB，localStorage 約 5MB 上限很快耗盡。

**改法要求**：
- 存檔僅保留 `{ 卡片ID（或卡名）, 張數 }` 與套牌名稱、時間戳
- 載入時透過 `/cards/collection` 批次重新取得完整資料（沿用既有 75 張分批邏輯）
- **必須提供舊格式自動遷移**：偵測到舊格式存檔時無痛轉換，不可讓使用者現有存檔遺失

## 三、功能強化（依序實作，時間不夠可停在任一項）

1. **匯入格式擴充**：支援 Arena 格式（`4 Card Name (NEO) 123`，需剝除集數與編號）、MTGO `4x Card Name`、以及 `Sideboard` 段落標記
2. **備牌（Sideboard）區**：獨立區域，支援主備牌互移，匯入/匯出同步支援
3. **起手抽牌模擬**:按鈕隨機抽 7 張顯示卡圖，可重抽、可調度（mulligan 遞減）
4. **指揮官判定強化**：顏色標識（color identity)檢查、100 張總數檢查、指揮官指定欄位

## 四、工程整理

- 將 `mtg-deck-editor.html` 複製為 `index.html`，讓 repo 根網址直接可用（保留原檔名以維持既有連結）
- 檢視三欄式排版在窄螢幕的表現，加入基本 RWD（可接受手機版為簡化版面）

## 五、約束與驗收

- **版本安全**：開新 branch 作業，逐項 commit，每項完成後手動驗證再進下一項
- **Scryfall 禮儀**:批次請求維持循序 await，各請求間加入 75–100ms 延遲
- 不破壞既有功能：搜尋、篩選、圖表、價格更新、匯出三種格式均需回歸測試
- 每完成一項，簡述改動內容與測試方式

## 驗收清單

- [ ] 匯入含錯誤卡名的清單，張數不錯位且有失敗提示
- [ ] 舊存檔可正常開啟（自動遷移）
- [ ] 新存檔體積大幅縮小（單副套牌 < 10KB）
- [ ] Arena 格式匯入成功
- [ ] 根網址 `lunar1989331.github.io/mtg-deck-editor/` 直接開啟編輯器
