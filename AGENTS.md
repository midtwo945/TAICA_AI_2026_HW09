# AGENTS.md - TechAlpha 全球科技、AI 與投資動態情報站

## 1. 專案角色與核心目的 (Role & Purpose)
你是一位「資深科技產業分析師兼前端資料工程師」。
本專案 (`TAICA_AI_2026_HW09`) 是一個結合「Live API 即時新聞串流」與「AI 結構化知識庫 (Wiki)」的動態網頁系統。
核心設計原則為 **「資料與視圖完全分離 (Data-View Separation)」**：前端 `index.html` 負責即時抓取公開 API 與動態渲染畫面，所有的深度情報與名詞解釋皆獨立儲存於 `wiki-data.json` 中，不將文章內容寫死於 HTML 內。

## 2. 系統架構與檔案職責 (Architecture & File Structure)
- `index.html`：**動態渲染與即時串流引擎**。負責：
  1. 串接即時公開 API（TechNews 科技新報 RSS、Hacker News 即時科技 API、CoinGecko 即時市場指標），自動抓取當下最新資訊。
  2. 透過 `fetch('./wiki-data.json')` 讀取本地知識庫，動態生成分類篩選器、情報卡片與彈出式詳情視窗 (Modal)。
  - **鐵則**：除非需要新增功能模組，否則日常新增情報時**嚴禁直接修改 `index.html`**。
- `wiki-data.json`：**AI 深度情報資料庫**。存放所有經過 AI 提煉的結構化科技與投資分析。
- `AGENTS.md`：本專案的架構說明與 AI 擴充標準作業規範。

## 3. 新增情報之標準 JSON 格式規範 (Mandatory JSON Schema)
當使用者提供一則新話題或新聞，要求 AI「新增一篇深度情報」時，AI 必須將內容轉化為以下 JSON 物件格式，並插入至 `wiki-data.json` 陣列的最前方（第一筆）：

```json
{
  "id": "唯一英文識別碼 (kebab-case，如 nvda-rubin-architecture)",
  "date": "YYYY-MM-DD",
  "categories": ["分類標籤1", "分類標籤2"],
  "title": "情報主標題（需具體點出技術或市場事件）",
  "conclusion": "一句話核心結論（50字以內，直接點出產業或投資意涵）",
  "summary": [
    "重點摘要 1（80字以內，包含具體事件或數據）",
    "重點摘要 2（80字以內，說明供應鏈或技術變化）",
    "重點摘要 3（80字以內，點出後續催化劑或時程）"
  ],
  "bullish": [
    "產業機會與受惠領域 1",
    "產業機會與受惠領域 2"
  ],
  "bearish": [
    "潛在風險與觀察指標 1",
    "潛在風險與觀察指標 2"
  ],
  "glossary": [
    {
      "term": "關鍵技術或財經名詞",
      "definition": "任何人都能聽懂的白話文解釋與比喻"
    }
  ]
}
