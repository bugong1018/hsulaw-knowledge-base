# Tips & Tricks

基於 Andrej Karpathy 的 [llm-wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 建議，針對法律知識庫的使用技巧。

## Obsidian 整合

### Web Clipper
安裝 [Obsidian Web Clipper](https://obsidian.md/clipper) 瀏覽器擴充功能，可將網路法律文章、判決、新聞一鍵轉為 markdown 存入 `raw/articles/`。

### 圖片下載
在 Obsidian 設定 → 檔案與連結 → 附件資料夾路徑，設定為 `raw/assets/`。設定快捷鍵（建議 Ctrl+Shift+D）給「下載目前檔案的附件」。使用 Web Clipper 抓取文章後，按快捷鍵將圖片下載到本地。這樣即使來源文章被刪除，wiki 中的引用仍然有效。

### Graph View
Obsidian 的圖譜視圖是查看知識庫結構的最佳工具——哪些概念是 hub、哪些頁面孤立無援、判決和法規之間如何透過概念連結。

### Dataview 插件
如果 LLM 在 wiki 頁面的 YAML frontmatter 中加入了結構化欄位（tags, jurisdiction, practice_area, date），Dataview 可以產出動態表格。例如，列出所有 `jurisdiction: TW` 且 `practice_area: trade-secrets` 的判決摘要。

### Marp 簡報
[Marp](https://marp.app/) 是 markdown 轉簡報的工具，Obsidian 有對應插件。可以直接從概念條目生成法律簡報。

## CLI 工具

### 搜尋：qmd
[qmd](https://github.com/tobi/qmd) 提供本地 markdown 搜尋，支援 BM25/向量混合搜尋與 LLM 重新排序。當知識庫超過 ~100 頁時，index.md 的目錄式導航可能不夠用，qmd 提供更精準的搜尋。

安裝（macOS）：`brew install qmd`
安裝（Linux）：`cargo install qmd`
MCP server 模式：`qmd serve --mcp`

### 時間序瀏覽
```bash
# 最近 10 筆操作
grep "^## \[" log.md | tail -10

# 查特定日期的所有操作
grep "^## \[2026-05" log.md

# 只看 ingest 操作
grep "^## \[.*\] ingest" log.md
```

### Git diff 查看變更
```bash
# 上次編譯改了多少頁面
git diff --stat HEAD~1

# 哪個概念被修改了
git log --oneline -- wiki/concepts/
```

## 法律特定技巧

### 判決摘要的「對實務的影響」欄位
這是最有法律價值的欄位——不只是摘要判決，而是把判決轉化為可執行的實務建議。這部分應該在與律師討論後撰寫，而非純靠 LLM 生成。

### 法規版本追蹤
每次法規修正後，將新舊版本都保存：
- `raw/regulations/2025-01-01-營業秘密法-v2025.md`（新版）
- `raw/regulations/2024-06-01-營業秘密法-v2024.md`（舊版保留）

在 `log.md` 中記錄修正重點。健康檢查時要掃描法規是否過時。

### 保密素材的處理
涉及客戶案件的素材：
- 去識別化後放入 `raw/cases/`
- 案件策略討論放在 `brainstorming/chat/`，標記 `confidential: true`
- 不要將 `confidential: true` 的內容存到公開 GitHub repo
- 考慮將 `brainstorming/chat/` 和 `raw/projects/` 加入 `.gitignore`，改用本地備份

### 跨管轄權比較
當同一概念在不同司法管轄區有不同定義時（如「合理保密措施」在台灣 vs 美國 vs 歐盟），概念條目的「管轄權差異」段落是關鍵——它讓你在跨國案件中快速掌握各地差異。
