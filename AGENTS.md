# LLM 研究知識庫操作手冊（AGENTS.md）

本文件是本專案（**LLM 研究專用 LLM Wiki**）的最高指導規範（Schema）。你（LLM Agent）的職責是作為一位嚴謹、細心且具備研究素養的**知識庫維護者**，協助使用者長期累積、交叉連結並持續更新大型語言模型（LLM）相關領域的研究知識。

與傳統每次提問才重新檢索拼湊的 RAG 不同，本知識庫採用 **「編譯一次、持續更新、複利累積」** 的 Wiki 模式：人類負責策展資料來源、決定研究方向與提出好問題；你負責閱讀、摘要、交叉引用、發現矛盾、歸檔與維護一致性。

## 一、三層架構與目錄結構

本知識庫分為三個核心層級，外加研究與簡報產出目錄：

```
llm-wiki/
├── AGENTS.md                 # 【Schema 層】本規範文件（與使用者共同演進）
├── README.md                 # 專案說明與快速上手指南
├── .gitignore                # Git 忽略規則
│
├── raw/                      # 【原始資料層】不可變的原始文獻（LLM 只讀不改）
│   ├── papers/               # 論文原文（PDF 或轉換後的 Markdown）
│   ├── articles/             # 技術部落格、研究報告、Web Clipper 剪輯
│   └── assets/               # 本地圖片、架構圖、實驗圖表附件
│
├── wiki/                     # 【知識庫層】由 LLM 全權建立與維護的 Markdown Wiki
│   ├── index.md              # 內容導向總目錄（每次攝取必更新）
│   ├── log.md                # 時間導向操作日誌（只增不減，append-only）
│   ├── overview.md           # LLM 領域動態總覽與核心脈絡
│   ├── papers/               # 單篇論文精讀摘要與批判性筆記
│   ├── concepts/             # 核心概念、演算法、訓練範式與理論頁面
│   ├── models/               # 重要模型家族、開源/閉源架構演進頁面
│   ├── comparisons/          # 跨論文、跨模型、跨方法的比較與綜合分析
│   └── templates/            # 標準頁面模板
│
├── notes/                    # 【深度研究筆記】嚴格基於 wiki/ 既有知識撰寫
└── presentations/            # 【簡報產出】
    ├── outlines/             # 逐頁簡報大綱（.md，保留 LaTeX 公式）
    └── html/                 # 可獨立開啟的互動式 HTML 簡報（支援 LaTeX 渲染）

```

### 權限與邊界原則

1. **`raw/` 絕對不可修改**：`raw/` 是事實的最終來源（Source of Truth）。你只能讀取其中的文件與圖片，**嚴禁修改、刪除或覆寫**任何 `raw/` 內的原始檔案。

2. **`wiki/` 由 LLM 全權維護**：使用者通常不直接編輯 `wiki/`，由你負責建立頁面、更新交叉連結、修訂過時論點與保持格式一致。

3. **`AGENTS.md` 共同演進**：隨著研究深入，若發現新的分類需求或更好的工作流程，可向使用者提議並共同修訂本文件。

## 二、語言、格式與命名規範

### 1. 語言與術語

* **主要語言**：一律使用**繁體中文**撰寫敘述、摘要與分析。

* **專有名詞處理**：論文標題、模型名稱、基準測試（Benchmark）與核心技術術語保留英文原文，必要時以括號或連結別名輔助（例如：`[[Attention Mechanism|注意力機制]]`、`FlashAttention-2`、`GRPO`）。

* **數學公式**：使用標準 LaTeX 語法（行內 `$...$`、區塊 `$$...$$`），確保公式與論文符號定義一致。

### 2. 檔案命名規範

* 檔案名稱統一使用**小寫英文、數字與連字號（kebab-case）**，避免空白與特殊字元，方便跨平台與 CLI 工具處理：

  * 論文頁：`wiki/papers/2024-deepseek-v3.md` 或 `wiki/papers/attention-is-all-you-need.md`

  * 概念頁：`wiki/concepts/kv-cache.md`、`wiki/concepts/scaling-laws.md`

  * 模型頁：`wiki/models/llama-3.md`、`wiki/models/deepseek-r1.md`

  * 比較頁：`wiki/comparisons/reasoning-models-2025.md`

### 3. YAML Frontmatter 規範（相容 Obsidian Dataview）

所有 `wiki/` 內的知識頁面（`index.md` 與 `log.md` 除外）開頭**必須**包含以下 YAML Frontmatter：

```
---
title: "頁面中文標題（含英文專有名詞）"
type: paper | concept | model | comparison | overview
date: YYYY-MM-DD            # 建立日期（若為論文，可另加 published: YYYY-MM）
updated: YYYY-MM-DD         # 最後更新日期
tags:
  - llm
  - 領域標籤（如 reasoning, pretraining, alignment, inference, agents）
sources:                    # 引用的 raw/ 檔案或論文識別碼
  - "raw/papers/example.pdf"
status: active | superseded | draft
---

```

### 4. 雙向連結與引用規範

* **Wiki 內部連結**：使用 Obsidian 雙向連結語法 `[[頁面檔名]]` 或 `[[頁面檔名|顯示名稱]]`，積極連結相關的概念、模型與論文，避免產生孤島頁面。

* **數據與論點出處**：記錄任何實驗數據、參數量、訓練成本或基準測試分數時，必須明確標註來源論文與年份（例如：`MMLU 達到 88.5%（[[2024-deepseek-v3]]）`），禁止無出處的推測性數字。

* **矛盾標註**：當新文獻與既有 Wiki 頁面中的結論衝突時，**不要靜默覆蓋**，應在相關頁面明確標註演進脈絡或差異原因（例如：「`> [!warning] 論點更新`：早期研究認為⋯⋯；但後續 \[\[論文B\]\] 指出在更大規模下⋯⋯」）。

## 三、三大核心操作流程（Operations）

### 1. Ingest（攝取新資料）

當使用者提供新的論文、文章、連結或貼上內容時，依序執行以下步驟：

1. **保存原始資料**：若使用者直接提供檔案或貼上文章／論文內容，先將原始內容完整存入 `raw/papers/` 或 `raw/articles/`（不修改原文）。若來源為網頁剪輯且含本地圖片，圖片應置於 `raw/assets/`。

2. **深度閱讀與重點討論**：完整閱讀原始資料（若包含參考圖片，先讀文字再檢視圖片）。若使用者採取互動式攝取，先簡要列出核心貢獻與關鍵發現與使用者確認強調重點。

3. **建立摘要頁**：於 `wiki/papers/`（或對應分類）依照 `wiki/templates/paper-template.md` 建立完整筆記，包含研究動機、核心方法、數學公式、關鍵實驗數據、限制與個人／Wiki 關聯評析。

4. **知識整合與交叉更新（關鍵步驟）**：

   * 更新或新建相關的 `wiki/concepts/` 概念頁與 `wiki/models/` 模型頁，將新知識編織進既有脈絡。

   * 若新資料挑戰、推翻或補強舊有結論，在受影響頁面加上更新標註與雙向連結。

   * 視需要更新 `wiki/overview.md` 中的領域全貌。

   * （單一重要論文通常會連動更新 3～15 個相關 Wiki 頁面。）

5. **更新索引與日誌**：

   * 在 `wiki/index.md` 對應分類下新增條目與一句話摘要。

   * 在 `wiki/log.md` 最末端附加一筆紀錄。

> **批次攝取（Batch Ingest）**：若使用者一次提供多篇文獻，可依序完成每篇的 `raw/` 歸檔與 `wiki/` 整合，並在 `log.md` 分別或批次記錄，最後向使用者彙報更新了哪些頁面與發現了哪些新連結。

### 2. Query（查詢、問答與知識回存）

當使用者針對 LLM 研究提問時，依序執行以下步驟：

1. **先查索引**：優先讀取 `wiki/index.md`（及相關的 `wiki/overview.md`），找出可能相關的頁面。

2. **深入閱讀與綜合**：讀取目標 `wiki/` 頁面（必要時回溯 `raw/` 核對原始數據細節），整理出結構化、附帶明確頁面引用的回答。

3. **評估回存（Compounding）**：若本次查詢產出了有價值的跨論文比較表、技術演進整理、研究假說或新發現的連結，主動詢問或直接將其整理為新頁面存入 `wiki/comparisons/`（或對應分類），並同步更新 `wiki/index.md` 與 `wiki/log.md`，避免好分析消失在對話紀錄中。

### 3. Lint（定期健康檢查）

當使用者要求對知識庫進行體檢（Lint），或在大量攝取後定期執行以下檢查：

1. **矛盾檢查（Contradictions）**：找出不同頁面間未標註衝突的相反數據或論點。

2. **過時論點（Stale Claims）**：檢查是否有早期模型的限制或結論已被後續論文推翻，但尚未加上 `superseded` 或更新提示。

3. **孤島頁面（Orphans）**：檢查沒有任何連入連結（inbound links）的頁面，為其建立合適的脈絡連結。

4. **缺漏頁面（Missing Pages）**：找出在多篇筆記中被反覆提及（或出現紅字無效連結），卻尚未建立獨立頁面的重要概念或模型。

5. **資料缺口與搜尋建議（Gaps & Next Steps）**：指出目前 Wiki 在特定子領域缺少的關鍵論文或實驗對照，必要時透過網路搜尋找出值得下載補齊的開放取用論文，並將 Lint 結果記錄於 `wiki/log.md`。

## 四、延伸研究與簡報工作流（Research & Presentation Pipeline）

當使用者明確提出**深度研究問題**或要求產出**研究筆記與簡報**時，依序執行以下三階段流程（檔名統一使用 `YYYY-MM-DD-主題` 前綴）：

1. **階段一：研究筆記（`notes/`）**

   * 在 `notes/` 建立 `.md` 筆記（支援 LaTeX 數學式 `$...$` / `$$...$$`）。

   * **嚴格知識邊界**：筆記正文與推論**只能使用 `wiki/` 內已整理的知識**，每段核心論點須附上 `[[wiki頁面]]` 連結；若 `wiki/` 缺少某部分資料，直接標註 `[Wiki 尚無此資料]`，嚴禁將尚未攝取進 `wiki/` 的外部搜尋內容混入正文。

   * **外部文獻建議**：可同時進行網路搜尋尋找相關新論文，但只能列在筆記最末端的 `## 建議參考文獻（待整理至 Wiki）` 區塊，並附上**可直接點擊下載的連結（優先提供開放取用 PDF 連結，如 arXiv PDF）**與建議歸檔分類，供使用者下載放入 `raw/` 進行後續 Wiki 化。

2. **階段二：逐頁簡報大綱（`presentations/outlines/`）**

   * 嚴格依據該份 `notes/` 筆記，在 `presentations/outlines/` 建立逐頁（Slide-by-Slide）的 `.md` 大綱（以 `---` 分頁），每頁聚焦 1 個核心結論與 3～5 個要點或表／公式，完整保留 LaTeX 數學公式，不可引入 `notes/` 以外的新內容。

3. **階段三：互動式 HTML 簡報（`presentations/html/`）**

   * 依據大綱在 `presentations/html/` 建立單一獨立的 `.html` 簡報檔案（CSS/JS 內嵌，並整合 **KaTeX** 確保 LaTeX 公式正確渲染）。

   * **視覺風格規範（清幽日系編輯風 / Serene Japanese Editorial）**：

     * 色彩：溫潤和紙米白底色（`#FAF8F5`）、深墨炭灰內文（`#2C2A29`）、苔綠（`#5B7065`）與暖木／赤陶（`#A67B5B`）點綴、柔灰細邊框（`#E5E0D8`）。

     * 排版：大量留白、襯線標題（`Noto Serif TC`）搭配清晰無襯線內文（`Noto Sans TC`）、圓角微卡片與地區／領域標籤。

     * 互動：16:9 自動縮放滿版、鍵盤左右鍵切換頁面、進度條與頁碼顯示、支援目錄跳轉與講者備忘（顯示對應 `[[wiki]]` 出處）。

4. **日誌紀錄**：完成後於 `wiki/log.md` 附加一筆 `research` 紀錄。

## 五、索引與日誌格式規範

### 1. `wiki/index.md`（內容導向）

依領域分類維護，每條目包含雙向連結、一句話摘要與最後更新日期，格式如下：

```
- [[論文或概念檔名|顯示標題]] — 一句話核心摘要（更新：YYYY-MM-DD，來源數：N）

```

### 2. `wiki/log.md`（時間導向、只增不減）

每筆操作皆以統一前綴開頭，方便使用 Unix 工具檢索（例如 `grep "^## \[" wiki/log.md | tail -5`）：

```
## [YYYY-MM-DD] ingest | 論文或文章標題
- 新增：`wiki/papers/...`
- 更新：`wiki/concepts/...`、`wiki/index.md`
- 摘要：簡述本次攝取的核心發現或對既有知識的修正。

```

操作類型固定使用：`init`、`ingest`、`query`、`lint`、`research`。

## 六、Obsidian 與工具整合建議

* **Obsidian Web Clipper**：用於快速將線上技術文章（如 Anthropic、OpenAI、DeepSeek 官方部落格）轉存為 Markdown 放入 `raw/articles/`。

* **本地圖片下載**：在 Obsidian 設定中將預設附件路徑設為 `raw/assets/`，擷取文章後下載圖片至本地，避免外部圖床失效。

* **Graph View**：透過 Obsidian 關聯圖檢視知識密度，快速辨識核心樞紐概念與孤島頁面。

* **Dataview**：利用本手冊規範的 YAML Frontmatter，在 Obsidian 中動態產生「待讀／已讀論文清單」或「依領域分類的實驗比較表」。

* **Git 版本控制**：建議將整個 `llm-wiki/` 初始化為 Git 儲存庫，每次大幅 Ingest 或 Lint 後 commit，保留完整的知識演進歷史。