# 架構：四層法律知識系統

## 為什麼是四層？

法律實務中，原始判決、法規、學說與你個人的法律判斷不應該混在一起。一份最高法院判決和你在讀完後的訴訟策略筆記是不同性質的東西——前者是客觀事實，後者是專業判斷。

這個系統強制維持明確分層：

```
raw/            → 法律圖書館     （判決、法規、學說、研討會資料）
wiki/           → 法律百科全書   （LLM 整理的摘要、概念、交叉引用）
brainstorming/  → 案件分析筆記本 （策略推理、爭點分析、QA）
artifacts/      → 事務所成果     （法律意見書、文章、簡報、交付物）
```

## 第 1 層：`raw/` — 法律圖書館

**規則：收進來之後就是唯讀。** 判決原文、法規條文、學說文章一旦存入 `raw/`，就不再編輯。保留最原始的樣貌，作為後續法律分析的客觀基礎。

### 法律特定子資料夾

| 資料夾 | 內容 | origin |
|--------|------|--------|
| `cases/` | 去識別化判決、案例研究 | external |
| `regulations/` | 法規、修正草案、主管機關函釋 | external |
| `articles/` | 法律文章、部落格、新聞 | external |
| `books/` | 法律書籍筆記、章節摘要 | external |
| `papers/` | 學術論文、法律期刊 | external |
| `podcasts/` | 法律 Podcast 筆記 | external |
| `notes/` | 個人觀察、研討會筆記 | self |
| `projects/` | 案件與專案原始材料 | self |

### 法律素材必要標記

每個 `raw/` 下的檔案應在檔名或內部標記：
- `jurisdiction:` — 管轄權（TW/CN/US/EU/JP...）
- `practice_area:` — 領域標籤
- `date:` — 原始日期（判決日、發布日、出版日）
- 法規另加：`effective_date:`、`last_amended:`

## 第 2 層：`wiki/` — 法律百科全書

**規則：由 LLM 維護，不手動編輯。**

### 摘要（`wiki/summaries/`）

每份來源一份摘要。格式依 `origin` 區分：

**External（判決、法規、學說）：**
- 核心結論 Core Conclusion
- 關鍵論據 Key Evidence / Ratio Decidendi
- 疑點與未解問題 Open Questions
- 關鍵術語 Key Terms
- 對實務的影響 Practice Implications（法律自訂欄位）

**Self（個人筆記、案件分析）：**
- 我的主張 My Claims
- 實務經驗 Practice Experience
- 未解決問題 Unresolved Questions
- 與外部觀點的比較 Comparison with Research

### 概念（`wiki/concepts/`）

當某個法律概念在 2 篇以上摘要中出現時，建立概念條目。結構：
- 定義（附管轄權差異說明）
- 我的實踐
- 外部觀點（判決、學說）
- 張力與缺口（判決分歧、學說爭議、實務困境）
- 相關法規
- 來源

### 索引（`wiki/indexes/`）

- **All-Sources.md** — 所有已編譯來源，附標籤與關鍵收穫
- **All-Concepts.md** — 所有概念條目，附定義與相關概念

## 第 3 層：`brainstorming/` — 案件分析筆記本

**規則：放探索過程，不放最終成果。**

- `chat/` — 法律策略推理、爭點分析、假設性問答
- `health/` — 知識庫健康檢查（法規版本是否過時、連結是否失效）

## 第 4 層：`artifacts/` — 事務所成果

**規則：放完成品。** 法律意見書、文章、簡報、客戶交付物。

這些內容也會被編譯進 `wiki/`，`origin: self`，使你的實務經驗與外部研究一起進入概念條目。

## 編譯流程

```
raw/cases/supreme-court-2025-trade-secret.md
        ↓ /compile
wiki/summaries/20250615 最高法院營業秘密判決.md
        ↓ concept extraction
wiki/concepts/營業秘密合理保密措施.md  (created or updated)
        ↓ index update
wiki/indexes/All-Sources.md    (new row added)
wiki/indexes/All-Concepts.md   (new row if new concept)
```

## `origin` 在法律知識庫中的意義

法律實務中，`origin` 的區分格外重要：

- **External**：判決、法規、學說——這是客觀法律事實
- **Self**：你的案件策略、訴訟經驗、法律判斷——這是專業智慧

兩者分開保存，才能在概念條目中清楚區分「法律怎麼說」和「我怎麼做」。這對律師的事後回顧與知識傳承尤其重要。
