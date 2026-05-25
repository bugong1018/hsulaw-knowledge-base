# Log

Chronological record of all wiki operations. Append-only.
Each entry starts with `## [YYYY-MM-DD] <operation> | <summary>` — parseable with grep.

Usage: `grep "^## \[" log.md | tail -10` → last 10 entries.

---

## [2026-05-25] init | Repository created

Initialized knowledge base with four-layer architecture. No sources ingested yet.

## [2026-05-25] ingest | 112年度刑智上重訴字第9號 — 違反營業秘密法等

Ingested IP & Commercial Court judgment on trade secrets. Key finding: company had security systems (ERP, NDA, access control) but failed to enforce them during shareholder inspection — "reasonable secrecy measures" not established. Appeal dismissed, not guilty affirmed.

- Summary: `wiki/summaries/20260305-IP法院-合理保密措施-股東查帳.md`
- Concept created: `wiki/concepts/合理保密措施.md` (first instance — meets 2-source threshold with future summaries)
- Raw source: `raw/cases/2026-03-05-智慧財產法院-刑智上重訴9-營業秘密.md`
- Index updated.
- Pages touched: 1 summary, 1 concept, index.md, log.md
