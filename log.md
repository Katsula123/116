# 操作日誌（Log）

> 本頁為**時間導向**的只增不減（append-only）紀錄，追蹤知識庫每次 `init`、`ingest`、`query`、`lint` 與 `research` 操作。
> 可使用 `grep "^## \[" wiki/log.md | tail -5` 快速檢視近期紀錄。

## [2026-11-12] init | 初始化 LLM 研究知識庫
- 新增：`AGENTS.md`、`README.md`、`.gitignore`、`wiki/index.md`、`wiki/log.md`、`wiki/overview.md` 及 `wiki/templates/` 模板組。
- 建立目錄結構：`raw/`（`papers/`、`articles/`、`assets/`）、`wiki/`（`papers/`、`concepts/`、`models/`、`comparisons/`、`templates/`）、`notes/`、`presentations/`（`outlines/`、`html/`）。
- 摘要：依據 Karpathy LLM Wiki 模式與繁體中文研究規範完成知識庫初始化。

## [2026-11-12] ingest | The Best of NIPPON47！47都道府縣「溫泉」特輯
- 保存原始資料：`raw/articles/2026-11-12-gltjp-nippon47-hot-spring.md`（來源：https://www.gltjp.com/zh-hant/nippon47/hot-spring/）
- 新增：`wiki/articles/gltjp-nippon47-hot-spring.md`、`wiki/concepts/japan-onsen-classifications.md`、`wiki/comparisons/japan-47-prefectures-onsen.md`
- 更新：`wiki/index.md`、`wiki/log.md`
- 摘要：完成首篇網頁測試攝取，保存原文至 `raw/articles/` 並建立 3 篇相互連結的 Wiki 頁面（涵蓋 47 都道府縣代表溫泉與日本名泉／古湯／美肌之湯分類）。
