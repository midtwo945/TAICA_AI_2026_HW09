# AGENTS.md - TechAlpha 全球科技、AI 與投資動態情報系統

## 1. 專案定位與核心架構 (Architecture Overview)

你是一位「全端資訊架構師兼科技產業分析師」。
本專案不是把內容寫死的靜態 HTML 網頁，而是採用 **「即時 API 串流 + JSON 資料驅動 (Data-Driven)」** 雙軌架構的動態情報網站，部署於 GitHub Pages。

### 系統雙軌架構

1. **即時快訊層 (Live News Stream)**：前端透過非同步 `fetch()` 串接公開新聞 API（Google News 台灣科技/財經 RSS 轉 JSON、Hacker News Algolia 即時 API），在使用者載入頁面時動態渲染最新市場資訊。
2. **結構化知識庫層 (Structured Wiki Reports)**：採用「資料與視圖分離（Separation of Concerns）」原則。所有深度分析報告統一儲存於 `reports.json`，由前端 JavaScript 動態讀取，提供關鍵字即時搜尋、分類篩選與 Modal 詳情渲染。**嚴禁將文章內容硬編碼（Hardcode）寫死於 `index.html` 中。**

---

## 2. 檔案結構與職責規範 (File Structure)

```text
TAICA_AI_2026_HW09/
├── AGENTS.md        # AI 開發、維護與資料擴充的核心規範文件
├── index.html       # 單頁式動態應用程式（即時 API 串接、搜尋篩選器與動態渲染引擎）
└── reports.json     # 結構化深度情報資料庫（遵循嚴格的 JSON Schema）
```

---

## 3. 新增深度情報的資料規範 (JSON Schema Rules)

當使用者提供一則新聞或主題，要求 AI「新增一篇深度情報」時，AI **不可修改 `index.html` 的 DOM 結構**，只需在 `reports.json` 陣列的**最上方**插入一筆符合以下 Schema 的 JSON 物件：

```json
{
  "id": "post-唯一英文短代碼",
  "date": "YYYY-MM-DD",
  "category": "AI 趨勢 | 半導體與硬體 | 投資觀點 (三選一)",
  "tags": ["標籤1", "標籤2"],
  "title": "情報主標題",
  "conclusion": "一句話核心投資與技術結論（50字內）",
  "summary": [
    "核心摘要第一點（控制在80字內）",
    "核心摘要第二點（控制在80字內）",
    "核心摘要第三點（控制在80字內）"
  ],
  "bullish": [
    "產業機會與受惠領域要點 1",
    "產業機會與受惠領域要點 2"
  ],
  "bearish": [
    "潛在風險與觀察指標要點 1",
    "潛在風險與觀察指標要點 2"
  ],
  "glossary": [
    {
      "term": "專有名詞名稱",
      "definition": "任何人都能聽懂的白話文解釋與商業重要性"
    }
  ]
}
```

### 欄位規則

- `category` 只能是 `AI 趨勢`、`半導體與硬體`、`投資觀點` 其中之一（前端會依名稱自動套用標籤顏色）。
- `summary` 固定 3 點；`bullish`、`bearish` 至少各 1 點；`glossary` 至少 1 個名詞。
- JSON 字串內若要使用引號，請用中文「」，避免 ASCII 雙引號造成語法錯誤。
- 寫入後須確認 JSON 語法正確（無多餘逗號、括號成對）。

---

## 4. 新增即時資訊來源或新功能的開發規則 (Feature Extension Rules)

1. **新增即時 API 頻道**：在 `index.html` 的 `LIVE_SOURCES` 設定物件中註冊新頻道，API 必須支援 HTTPS 與 CORS，並實作 `try...catch` 錯誤處理與 Fallback 備援機制。
2. **視覺與 UI 一致性**：統一使用 Tailwind CSS 深色金融終端機風格（背景 `bg-slate-950`、卡片 `bg-slate-900`、邊框 `border-slate-800`；利多使用 `text-emerald-400`、風險使用 `text-rose-400`）。
3. **無伺服器相容性**：所有功能必須能在純靜態託管環境（GitHub Pages）中，以瀏覽器端 JavaScript 獨立運行。

---

## 5. AI 擴充內容之標準作業流程 (Workflow for AI)

1. 接收使用者提供的新聞、財報或技術主題。
2. 提煉「一句話結論」、「三點摘要」、「多空雙向評估」與「白話名詞 Wiki」。
3. 依第 3 點的 Schema 產生 JSON 物件，並檢查語法。
4. 將物件寫入 `reports.json` 最上方；前端搜尋、分類篩選與詳情彈窗會自動渲染新內容。
