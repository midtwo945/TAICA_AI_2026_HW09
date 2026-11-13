# TechAlpha｜全球科技、AI 與投資動態情報系統

一個結合「即時新聞串流」與「AI 結構化知識庫」的靜態網站，用來追蹤全球科技、AI 與半導體投資動態。

本專案是 TAICA AI 2026 第九週作業（HW09）：設計一套讓 AI 能依規則持續擴充內容的專案，並以 `AGENTS.md` 作為 AI 的開發規範。

- 🌐 網站：https://midtwo945.github.io/TAICA_AI_2026_HW09/
- 📦 GitHub：https://github.com/midtwo945/TAICA_AI_2026_HW09

## 專案特色

- **即時快訊層**：網頁載入時透過 `fetch()` 串接 Google News RSS（經 rss2json 轉換）與 Hacker News Algolia API，顯示最新科技與財經新聞，RSS 失敗時自動改用 Hacker News 備援。
- **深度情報庫**：每篇報告包含一句話結論、三點摘要、利多與風險評估、白話名詞 Wiki，支援關鍵字搜尋、分類篩選與彈窗閱讀。
- **資料與畫面分離**：報告內容全部存放在 `reports.json`，`index.html` 不含任何文章文字，新增內容不需要修改 HTML。
- **AI 擴充指令產生器**：在即時新聞卡片按下「轉為 Wiki 報告」，會產生一段依照 `AGENTS.md` 撰寫的提示詞，可貼給 AI 協助新增報告。
- **純靜態部署**：不需要後端，可直接用 GitHub Pages 發布。

## 如何使用網站

網站分成上下兩個區塊，打開網頁後不需要登入或設定。

### 1. 即時快訊區（上半部）

1. 打開網頁後會自動抓取最新新聞，右上角的狀態燈顯示 API 是否連線正常（綠燈正常、紅燈失敗）。
2. 用上方的頻道按鈕切換來源：**台灣 AI 與半導體**、**美股與科技投資**、**全球 AI 動態（Hacker News）**。
3. 點新聞標題會在新分頁開啟原始報導。
4. 想看最新內容，按「🔄 重新整理」重新抓取。

### 2. 深度情報庫（下半部）

1. **搜尋**：在搜尋框輸入關鍵字（例如 `CoWoS`、`HBM`、`3nm`），會即時篩選標題、結論、標籤與摘要。
2. **分類篩選**：用下拉選單選擇「AI 趨勢」、「半導體與硬體」或「投資觀點」。
3. **閱讀完整報告**：點卡片標題或「展開完整分析」，會開啟彈窗，內容包含一句話結論、三點摘要、利多與風險、白話名詞解釋。
4. **關閉彈窗**：按「✕ 關閉」、按鍵盤 `Esc`，或點彈窗外的空白處。

### 3. 把新聞變成深度報告（AI 擴充指令產生器）

1. 在即時新聞卡片按「⚡ 依 AGENTS.md 轉為 Wiki 報告」（也可按右上角「AI 擴充指令產生器」自行輸入主題）。
2. 在彈窗中按「📋 一鍵複製指令」。
3. 把指令貼給 AI（建議使用能讀取本倉庫的工具，如 Cursor、Claude Code；一般聊天視窗請連同 `AGENTS.md` 內容一起貼上）。
4. 將 AI 產生的 JSON 放進 `reports.json` 最上方並 push，網頁重新整理後就會出現新卡片。

### 常見問題

- **顯示「無法讀取 reports.json」**：確認 `reports.json` 與 `index.html` 在同一層，且 JSON 語法正確；本機預覽請用 `python -m http.server`，不要直接雙擊開啟。
- **即時新聞顯示失敗**：可能是免費 API 暫時達到上限，稍後按「重新整理」，或切換到「全球 AI 動態」頻道。

## 檔案結構

```text
TAICA_AI_2026_HW09/
├── AGENTS.md      # AI 開發與內容擴充規範（專案規則的核心文件）
├── README.md      # 專案簡介（本文件）
├── index.html     # 動態渲染引擎：即時新聞、搜尋篩選、彈窗
└── reports.json   # 深度情報資料庫（遵循 AGENTS.md 的 JSON Schema）
```

## 如何新增一篇深度情報

1. 準備一則新聞或主題。
2. 請 AI 閱讀 `AGENTS.md`，並依第 3 點的 JSON Schema 產生一筆報告。
3. 將該 JSON 物件放進 `reports.json` 陣列的**最上方**。
4. commit 並 push，網站會自動顯示新卡片，不需要修改 `index.html`。

範例提示詞：

```text
請閱讀本專案的 AGENTS.md，針對以下新聞，在 reports.json 最上方新增一筆深度情報，
嚴格遵守 JSON Schema，不要修改 index.html。

【新聞主題】：（貼上新聞標題或內容）
```

## 如何新增即時新聞頻道

在 `index.html` 的 `LIVE_SOURCES` 物件中註冊新頻道即可，頻道按鈕會自動產生。API 需支援 HTTPS 與 CORS，詳見 `AGENTS.md` 第 4 點。

## 本機預覽

直接雙擊開啟 `index.html` 時，瀏覽器會阻擋讀取 `reports.json`。請在專案資料夾執行：

```bash
python -m http.server 8000
```

再開啟 http://localhost:8000。

## 技術

HTML、原生 JavaScript、Tailwind CSS（CDN）、GitHub Pages。

## 免責聲明

`reports.json` 內的報告為示範內容，未經完整查證，僅供課程作業展示，不構成任何投資建議。
