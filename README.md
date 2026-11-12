# LLM 研究知識庫（LLM Wiki）

本儲存庫採用 Karpathy 提出的 **LLM Wiki** 模式建立，專注於大型語言模型（LLM）相關論文、架構、訓練範式與推理技術的長期累積與系統化整理。

## 快速開始

1. **用 Obsidian 開啟本資料夾**：將 `llm-wiki/` 設為 Obsidian Vault，即可透過 Graph View、雙向連結與 Dataview 外掛瀏覽知識圖譜。
2. **用 LLM Agent 開啟本資料夾**：使用 Claude Code、Codex、OpenCode 或其他支援 `AGENTS.md` 的 Agent，它會自動讀取根目錄的 `AGENTS.md` 作為操作規範。
3. **開始攝取資料（Ingest）**：
   - 將論文 PDF 或文章 Markdown 放入 `raw/papers/` 或 `raw/articles/`。
   - 對 Agent 說：「請攝取 `raw/papers/xxx.pdf` 並更新 Wiki」。
4. **開始提問與研究（Query）**：
   - 對 Agent 說：「根據目前的 Wiki，比較 A 方法與 B 方法在長文本推理上的優缺點」。
5. **定期健康檢查（Lint）**：
   - 對 Agent 說：「請對 Wiki 執行一次 Lint 檢查，找出孤島頁面與矛盾論點」。

## 目錄說明

- `AGENTS.md`：LLM Agent 的核心規範與工作流程（Schema）。
- `raw/`：原始論文、文章與圖片（唯讀來源）。
- `wiki/`：由 LLM 維護的繁體中文知識庫（含 `index.md`、`log.md`、`overview.md` 及各分類目錄）。
- `notes/`：嚴格基於 `wiki/` 撰寫的研究筆記。
- `presentations/`：由研究筆記延伸產出的簡報大綱與 HTML 簡報。
