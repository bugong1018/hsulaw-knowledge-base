# AGENTS.md

這份文件提供 LLM agent 使用明毓知識庫時的操作規則。基於 gatelynch/llm-knowledge-base 架構，針對法律專業場景客製化。

## 先讀這些文件

- `docs/architecture.md`
- `docs/examples/summary-external.md`
- `docs/examples/summary-self.md`
- `docs/examples/concept-entry.md`

## 核心架構

這是一個四層知識庫，專為 明毓律師事務所 設計：

- `raw/`：原始素材，收進來後不再修改
  - `articles/` — 網路文章、部落格文章、新聞
  - `books/` — 書籍筆記、章節摘要
  - `podcasts/` — Podcast 逐字稿或筆記
  - `papers/` — 學術論文、法律期刊文章
  - `notes/` — 個人即時想法、研討會筆記（`origin: self`）
  - `projects/` — 與案件或專案相關的原始材料（`origin: self`）
  - `cases/` — 去識別化判決、案例研究（`origin: external`）
  - `regulations/` — 法規更新、立法動態、函釋（`origin: external`）
- `wiki/`：LLM 編譯後的知識，不手動維護
- `brainstorming/`：法律推理、策略探索、健康檢查等中間輸出
- `artifacts/`：事務所正式產出——文章、簡報、法律意見書

## 操作規則

- 不要修改 `raw/` 裡已存在的檔案內容
- 讀取新素材後，將摘要寫入 `wiki/summaries/`
- 只在概念出現在 2 份以上摘要時建立或更新 `wiki/concepts/`
- 將索引寫入 `wiki/indexes/All-Sources.md` 與 `wiki/indexes/All-Concepts.md`
- 將探索式法律推理與問答紀錄存入 `brainstorming/chat/`
- 將健康檢查報告存入 `brainstorming/health/`
- 將 `artifacts/` 下的內容視為 `origin: self`
- 將 `raw/articles/`、`raw/books/`、`raw/podcasts/`、`raw/papers/`、`raw/cases/`、`raw/regulations/` 視為 `origin: external`
- 將 `raw/notes/`、`raw/projects/` 視為 `origin: self`

## 法律特定規則

- **去識別化**：進入 `raw/cases/` 的案例必須去識別化（不得包含當事人姓名、公司名、案號）
- **管轄權標記**：所有素材標記 `jurisdiction:`（TW/CN/US/EU/JP 等）
- **法規時效**：法規素材標記 `effective_date:` 與 `last_amended:`
- **保密分級**：涉及客戶案件的分析放在 `brainstorming/chat/`，標記 `confidential: true`

## 五大執業領域標籤

所有素材和摘要應標記至少一個領域標籤：
- `tech-law` — 科技法
- `corporate-finance` — 企業與金融
- `intellectual-property` — 智慧財產權
- `trade-secrets` — 營業秘密
- `data-privacy` — 個資與隱私

## 工作流程對應

- `compile`：讀取 `raw/` 與 `artifacts/` 的新檔案，生成摘要、更新概念、更新索引
- `thinking partner`：針對法律難題提出結構化分析——事實→爭點→法規→判決趨勢→策略選項
- `write partner`：為法律文章或意見書做資料蒐集，找出相關判決、學說、反方觀點
- `braindump`：把法律對話沉澱成可重用素材，存到 `brainstorming/chat/`
- `health check`：檢查 `wiki/` 的一致性、完整性，特別注意法規版本是否過時
