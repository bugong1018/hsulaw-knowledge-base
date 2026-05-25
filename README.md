# 明毓知識庫 · Hsu & Associates Knowledge Base

以四層架構管理的法律知識庫，參考 Andrej Karpathy 的 LLM Wiki 概念與 gatelynch/llm-knowledge-base 架構。

A four-layer legal knowledge base for 明毓律師事務所 (Hsu & Associates), based on the LLM Wiki pattern by Andrej Karpathy and gatelynch/llm-knowledge-base.

## 四層架構 · Four Layers

| 層級 | 目錄 | 比喻 | 用途 |
|------|------|------|------|
| 1 | `raw/` | 圖書館 Library | 法律文章、判決、法規、研討會筆記等原始素材 |
| 2 | `wiki/` | 百科全書 Encyclopedia | LLM 編譯的摘要、概念條目、索引 |
| 3 | `brainstorming/` | 實驗筆記本 Lab Notebook | 法律策略探索、案件推理、QA 記錄 |
| 4 | `artifacts/` | 發表成果 Publications | 文章、簡報、客戶交付物、法律意見書 |

## 五大執業領域 · Five Practice Areas

- ⚡ **科技法** Technology Law
- 🏢 **企業與金融** Corporate & Finance
- 💡 **智慧財產權** Intellectual Property
- 🛡️ **營業秘密** Trade Secrets
- 🔒 **個資與隱私** Data Privacy & Protection

## 自訂資料夾 · Custom Directories

本知識庫在原架構基礎上新增：

- `raw/cases/` — 去識別化判決與案例研究
- `raw/regulations/` — 法規更新、立法動態、主管機關函釋

## 雙語支援 · Bilingual Support

所有摘要、概念條目均支援中文與英文，以 `lang: zh-TW` / `lang: en` 標記。
