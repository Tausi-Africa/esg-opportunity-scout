# ESG OpportunityScout

A **knowledge repository and Claude-driven automation skill** that scouts for ESG, climate-finance, and green-finance opportunities across **Tanzania, East Africa, and Southern Africa** on behalf of the consortium of **AfroPavo Analytics Ltd**, **BSM Washauri (TZ) Ltd**, and **Dentons EALC**.

This repo is not a conventional application. It is the **source of record** that a Claude routine reads on every run — the verified link registry, the consortium knowledge base, and the skill definition that tells Claude exactly how to search, verify, score, and report opportunities.

---

## What it does

On each run, the `esg-opportunity-scout` Claude skill:

1. **Reads the consortium knowledge files** in this repo (MoU, company profiles, team CVs, link registry) before searching anything.
2. **Searches 13 source categories** — climate funds, DFIs, UN agencies, regional bodies, national portals, bilateral donors, ESG networks, carbon markets, challenge funds, aggregators, foundations, innovation hubs, and think tanks — with targeted keyword combinations across the three target regions.
3. **Verifies every candidate against a live fetched page** (`web_fetch`) — zero hallucination. Dead, mismatched, or unverifiable URLs are excluded or explicitly flagged.
4. **Scores consortium relevance** (High / Medium / Low) and **assigns a lead partner** based on each firm's capabilities.
5. **Compiles a 22-column CSV**, uploads it to **Google Drive**, and sends a formatted **HTML email via Gmail** to the consortium working group.
6. **Optionally creates Google Calendar reminders** for high-relevance deadlines within 30 days.

The guiding principle is strict: **a short verified report beats a long unverified one.** Nothing is fabricated; unknown fields are written as `Not Found` / `Not Stated`.

---

## Directory structure

```
esg-opportunity-scout/
├── README.md                          # This file
├── instruction.md                     # Weekly run instructions for the Claude routine (10 ordered steps)
├── .gitignore
├── links.md                           # MASTER verified link registry (Priority-1 source list)
│
├── .claude/
│   └── skills/
│       └── esg-opportunity-scout/
│           └── SKILL.md               # The Claude skill: search → verify → score → report → deliver
│
├── template/
│   └── email_html_template.html       # CANONICAL email design (used verbatim by the routine)
│
├── knowledge-base/
│   ├── consortium-mou.md              # Consortium MoU — roles & collaboration areas
│   └── company-profiles/
│       ├── afropavo_company_data_a.md # AfroPavo overview, services, case studies
│       ├── afropavo_company_data_b.md # AfroPavo projects & references
│       ├── bsm-washauri-portfolio-of-projects.md  # BSM Washauri portfolio
│       └── dentons-ealc-profile.md    # Dentons EALC profile
│
└── team-cvs/
    ├── apa-cvs.md                     # AfroPavo team CVs / named experts
    ├── bsm-washauri.md                # BSM Washauri team CVs
    └── dentons-team.md                # Dentons team CVs
```

> **Planned / not yet present** (the skill flags these as missing until added): an `output/` directory for archived reports. `links.md` also doubles as the source registry — there is no separate `source_registry.md`.

---

## Key files

| File | Purpose |
|---|---|
| `instruction.md` | The **weekly run instructions** handed to the Claude routine — 10 ordered steps (tooling check → read skill → read knowledge → plan → search → verify → compile CSV → Drive → email → calendar → final report), each with a verification gate. Points at `SKILL.md` for the search universe, scoring, CSV schema, and email template. |
| `links.md` | The **master verified link registry** *(and source registry)* — Priority-1 source list searched before any default source. ~86 curated procurement/tender links across governments, regional bodies, climate finance, DFIs, UN agencies, embassies, foundations, and aggregators. Source reachability/status is tracked here too. |
| `.claude/skills/esg-opportunity-scout/SKILL.md` | The full operating manual for the Claude routine: startup checklist, strict accuracy rules, three-tier verification protocol, source universe, keyword sets, 22-column CSV schema, relevance scoring, response structure, and Drive/Gmail/Calendar delivery steps. Points to the canonical email template. |
| `template/email_html_template.html` | The **canonical email design** — single source of truth for the HTML report layout (white background, dark text with green accents, stat row, green opportunity cards, Drive button). The routine loads this and fills in each run's real values; it is never restyled inline. |
| `knowledge-base/consortium-mou.md` | Defines partner roles and collaboration areas — the basis for lead-partner assignment. |
| `knowledge-base/company-profiles/*` | Per-firm capability detail used to score opportunity fit. |
| `team-cvs/*` | Named experts and qualifications, matched against opportunity criteria. |

---

## The consortium

| Partner | Leads on |
|---|---|
| **AfroPavo Analytics** | Climate-finance instruments, green-finance structuring, data/AI/ML, applied research, impact metrics, MEL/MRV design |
| **BSM Washauri (TZ)** | Strategy, governance, policy, institutional development, stakeholder engagement, proposal structuring, training |
| **Dentons EALC** | Legal, regulatory, fiduciary, ESG-compliance frameworks, GCF transaction structuring |

**Geographic scope:** Tanzania (HQ — searched every run), East Africa (Kenya, Uganda, Rwanda, Burundi, South Sudan, DRC), and Southern Africa (South Africa, Mozambique, Zambia, Zimbabwe, Malawi, Namibia, Botswana, Angola, Lesotho, eSwatini, Madagascar, Mauritius), plus regional / multi-country programmes.

---

## How a run works (for Claude)

The full protocol lives in [`.claude/skills/esg-opportunity-scout/SKILL.md`](.claude/skills/esg-opportunity-scout/SKILL.md). In brief:

1. **Startup** — read every knowledge file; confirm or flag each at the top of the response. Never invent the contents of a missing file.
2. **Search** — work through `links.md` first, then the 13 source categories, combining keywords with each region term.
3. **Verify (3 tiers)** — existence (`web_fetch` the exact URL) → content extraction (fields only from the fetched page/PDF) → high-relevance corroboration on a primary source. Maintain a verification ledger.
4. **Compile** — produce the 22-column CSV; every field is real or `Not Found` / `Not Stated`.
5. **Deliver** — upload to Google Drive → send the HTML email (CSV attached, Drive link in body) → optionally create calendar reminders.

### Response sections (in order)
`<file_confirmation>` · `<tooling_status>` · `<search_summary>` · `<verification_ledger>` · `<opportunities_found>` · `<key_findings>` · `<csv_data>` · `<recommended_next_steps>` · `<google_drive_instructions>` · `<email_instructions>`

---

## Maintenance

- **Keep `links.md` current.** It is both the link registry and the source registry — when a source moves, dies, or becomes blocked, update its entry with the date and status.
- **Confirm recipients in writing** before adding anyone to the email distribution list. Note: `procurement@fsdt.or.tz` is an external bid-submission address and is **never** CC'd on the internal report.

---

## Repository of record

```
https://github.com/Tausi-Africa/esg-opportunity-scout
```

Claude reads all files from the repository, never from local disk.
