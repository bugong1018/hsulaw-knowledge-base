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

## [2026-05-25] ingest | 109年度刑智上重訴字第4號 — 美光/聯電 DRAM 營業秘密案

Ingested IP & Commercial Court landmark judgment on UMC/Micron DRAM trade secrets case. Key finding: Micron's technical enforcement measures (USB disabled by system, DLP auto-monitoring) established "reasonable secrecy measures" — in direct contrast to prior case where measures existed on paper but weren't enforced. UMC convicted as corporation under Article 13-4. Individual defendants received suspended sentences.

- Summary: `wiki/summaries/20220127-IP法院-美光聯電-DRAM營業秘密.md`
- Concept updated: `wiki/concepts/合理保密措施.md` — added 2nd source, enriched Tensions & Gaps with tech-vs-policy contrast
- Raw source: `raw/cases/2022-01-27-IP法院-刑智上重訴4-美光聯電-DRAM營業秘密.md` (partial — full 134K text at source URL)
- Index updated.
- Pages touched: 1 summary, 1 concept (updated), index.md, log.md
