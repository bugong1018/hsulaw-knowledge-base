# AGENTS.md — 明毓知識庫 LLM Agent 操作規則

本文件是 LLM agent 的 schema 設定檔。它與知識庫一起成長——當你發現更好的做法或遇到新的需求時，請和 agent 一起更新這份文件。

基於 Andrej Karpathy 的 [llm-wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 模式與 gatelynch/llm-knowledge-base 架構，針對法律專業場景客製化。

## 核心概念

這個系統的關鍵差異：**LLM 不是每次查詢時才從原始文件中擷取片段——而是在每次加入新來源時，就建立並維護一個持久的、結構化的知識網絡。** 摘要寫一次就保留，交叉引用已經建立，矛盾已經被標記，概念條目隨著每個新來源而不斷深化。知識是累積的，不是每次重新推導。

## 架構總覽

```
hsulaw-knowledge-base/
├── index.md              # 統一索引——所有 wiki 頁面的目錄
├── log.md                # 時間序操作紀錄——append-only, parseable
├── AGENTS.md             # 本文件——與知識庫共同演進的 schema
├── README.md             # 雙語總覽
├── docs/                 # 架構文件與範例
├── raw/                  # 第 1 層：原始素材（唯讀）
│   ├── articles/         #   external — 法律文章、部落格、新聞
│   ├── books/            #   external — 法律書籍筆記
│   ├── podcasts/         #   external — Podcast 筆記
│   ├── papers/           #   external — 學術論文、法律期刊
│   ├── cases/            #   external — 去識別化判決、案例研究
│   ├── regulations/      #   external — 法規、修正草案、函釋
│   ├── notes/            #   self    — 個人觀察、研討會筆記
│   └── projects/         #   self    — 案件與專案原始材料
├── wiki/                 # 第 2 層：LLM 編譯的知識（不手動編輯）
│   ├── summaries/        #   每個來源一份結構化摘要
│   ├── concepts/         #   跨來源的概念條目（出現 2+ 次才建立）
│   └── indexes/          #   (legacy — 統一索引已移至 index.md)
├── brainstorming/        # 第 3 層：探索與推理過程
│   ├── chat/             #   QA 記錄、法律策略推理
│   └── health/           #   知識庫健康檢查報告
└── artifacts/            # 第 4 層：事務所正式成果
    └── projects/         #   法律意見書、文章、簡報、交付物
```

## 操作流程

### 1. Ingest（加入新來源）

當你把新素材放入 `raw/` 後，agent 執行以下步驟：

1. **讀取並討論**：閱讀素材內容，與你討論關鍵收穫與重點
2. **撰寫摘要** → `wiki/summaries/YYYYMMDD 標題.md`
   - External 格式：核心結論 → 關鍵證據 → 疑點 → 術語 → 對實務的影響
   - Self 格式：我的主張 → 實務經驗 → 未解決問題 → 與外部觀點的比較
3. **更新概念條目** → 掃描摘要中的關鍵詞，若某詞在 ≥2 篇摘要中出現，建立或更新 `wiki/concepts/概念名.md`
4. **更新索引** → 在 `index.md` 的對應表格中加入新條目（一列、一行的摘要）
5. **寫入記錄** → 在 `log.md` 底部 append 一筆

一個來源可能觸及 10-15 個 wiki 頁面。

### 2. Query（查詢知識庫）

當你針對 wiki 提問時，agent 的流程：

1. **讀取 index.md** → 找到相關頁面
2. **鑽入相關頁面** → 讀取摘要、概念條目、brainstorming 記錄
3. **合成答案** → 附上引用（指向 wiki 頁面與原始 raw/ 檔案）
4. **將好答案存回 wiki**（重要！）：如果你問的比較、分析、或連結有價值，agent 應該主動問你：「這個分析要存回 wiki 嗎？」——答案的形式可以是新的概念條目、brainstorming/chat/ 記錄、或一個新的 artifacts/ 產出

### 3. Lint（健康檢查）

定期請 agent 檢查 wiki 的品質：

- 頁面之間是否有矛盾？
- 是否有舊論述被新來源推翻？
- 是否有孤兒頁面（無 incoming links）？
- 是否有重要概念被提及但缺少自己的頁面？
- 是否有遺漏的交叉引用？
- 法規版本是否過時？
- 是否有可以透過網路搜尋填補的知識缺口？

報告存入 `brainstorming/health/`。

## 連結規則（Bidirectional Linking）

這是知識庫的核心價值——**所有東西都要可以雙向追溯**。

### wiki → raw（每篇摘要必須鏈回原始素材）

在 `wiki/summaries/` 的 YAML frontmatter 中：

```yaml
source: "../raw/cases/2025-06-01-最高法院-營業秘密判決.md"
```

### concepts → summaries（每個概念必須列出所有來源摘要）

在 `wiki/concepts/` 的 YAML frontmatter 中：

```yaml
sources:
  - "../summaries/20250601 最高法院營業秘密判決.md"
  - "../summaries/20250610 離職員工訴訟策略.md"
```

### artifacts → wiki + raw（事務所成果必須引用知識來源）

在 `artifacts/` 的文件中：

```markdown
## 參考資料

### 判決與法規
- [最高法院營業秘密判決](../raw/cases/2025-06-01-最高法院-營業秘密判決.md)

### 本所知識
- [合理保密措施](../wiki/concepts/合理保密措施.md)
- [離職員工訴訟策略筆記](../wiki/summaries/20250610-離職員工訴訟策略.md)
```

### 所有連結使用相對路徑

- `wiki/summaries/` → `../../raw/cases/file.md`
- `wiki/concepts/` → `../summaries/file.md`
- `artifacts/` → `../wiki/concepts/file.md` 或 `../raw/cases/file.md`

## 五個執業領域標籤

所有素材和摘要必須標記 ≥1 個領域標籤：
- `tech-law` — 科技法
- `corporate-finance` — 企業與金融
- `intellectual-property` — 智慧財產權
- `trade-secrets` — 營業秘密
- `data-privacy` — 個資與隱私

## 法律特定規則

- **去識別化**：進入 `raw/cases/` 的案例必須去識別化（不得包含當事人姓名、公司名、案號）
- **管轄權標記**：所有素材標記 `jurisdiction:`（TW/CN/US/EU/JP...）
- **法規時效**：法規素材標記 `effective_date:` 與 `last_amended:`
- **保密分級**：涉及客戶案件的分析放在 `brainstorming/chat/`，標記 `confidential: true`

## 前處理規則

### 讀取 raw/ 新檔案時的處理

1. **檢查 `index.md`** — 這個素材是否已經處理過？
2. **檢查 `log.md`** — 最近有什麼被加入，有沒有相關脈絡？
3. **讀取素材全文**
4. **搜尋既有概念** — 素材中的關鍵詞是否已有對應概念條目？
5. **討論關鍵收穫** — 與你確認重點後再寫摘要
6. **生成摘要** — 存入 `wiki/summaries/`
7. **更新概念** — 跨來源概念建立或更新
8. **更新 `index.md`**
9. **Append `log.md`**
10. **報告** — 告訴你哪些概念被更新了、哪些是新建立的

## 檔案命名規則

- 摘要：`wiki/summaries/YYYYMMDD 簡短標題.md`
- 概念：`wiki/concepts/概念名.md`（中文概念用中文檔名）
- 判決 raw：`raw/cases/YYYY-MM-DD-法院-案由.md`
- 法規 raw：`raw/regulations/YYYY-MM-DD-法規名稱-版本.md`
- Brainstorming：`brainstorming/chat/YYYYMMDD-問題簡述.md`
- Health check：`brainstorming/health/YYYYMMDD-health-check.md`

## 與 Obsidian 相容

如果你使用 Obsidian 瀏覽知識庫：
- 所有 `[[wikilinks]]` 可以正常解析
- 相對路徑連結（`../raw/...`）也能在 Obsidian 中點擊
- 使用 Graph View 查看概念之間的關聯結構
- 推薦安裝 Dataview 插件，可以查詢 YAML frontmatter
