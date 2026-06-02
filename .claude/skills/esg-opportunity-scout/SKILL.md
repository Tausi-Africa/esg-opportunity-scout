---
name: esg-opportunity-scout
description: Runs a full ESG, climate-finance, and green-finance opportunity scan across Tanzania, East Africa, and Southern Africa for the AfroPavo / BSM Washauri / Dentons EALC consortium. Searches 13 source categories with targeted keyword combinations, verifies every URL and every field against live fetched pages (zero hallucination), scores consortium relevance, assigns a lead partner, compiles a 22-column CSV, archives it to Google Drive, sends a formatted HTML email via Gmail, and optionally creates calendar deadline reminders.
---

# ESG OpportunityScout — Green & Climate Finance Consortium

## What this skill does

Runs a weekly ESG / climate-finance / green-finance opportunity scan on behalf of the consortium of **AfroPavo Analytics Ltd**, **BSM Washauri (TZ) Ltd**, and **Dentons EALC**.

Each run: reads consortium knowledge files → searches 13 source categories with targeted keywords across **three regions** → collects candidates → **verifies every candidate against a live fetched page (`web_fetch`)** → scores consortium relevance and assigns a lead partner → compiles a 22-column CSV → **uploads it to Google Drive** → **sends a formatted HTML email via Gmail** (CSV attached, Drive link in body) → **optionally creates Google Calendar deadline reminders**.

This skill is the ESG successor to AfroPavo's general OpportunityScout. It is deliberately **more rigorous on verification** and **broader on source coverage**.

---

## REPOSITORY OF RECORD

```
https://github.com/Tausi-Africa/esg-opportunity-scout
```

Actual repository layout (verified 2026-06-01):

```
links.md                                                              <- MASTER verified link registry (Priority-1)
knowledge-base/consortium-mou.md                                      <- consortium MoU (roles, collaboration areas)
knowledge-base/company-profiles/afropavo_company_data_a.md            <- AfroPavo overview/services/case studies
knowledge-base/company-profiles/afropavo_company_data_b.md            <- AfroPavo projects/references
knowledge-base/company-profiles/bsm-washauri-portfolio-of-projects.md <- BSM Washauri portfolio
knowledge-base/company-profiles/dentons-ealc-profile.md               <- Dentons profile
team-cvs/apa-cvs.md · team-cvs/bsm-washauri.md · team-cvs/dentons-team.md  <- team CVs / named experts
```

To be added (flag as missing until present): `output/`.

Read all files from the repository, never from a local disk. **The repo is strictly read-only — never edit, write, commit, or push anything back to it.** The link registry is **`links.md` at the repo root** (not `additional_urls.txt`); it also serves as the source registry — there is no separate `source_registry.md`. The only outbound actions a run takes are the delivery steps (Drive upload, Gmail email, optional Calendar reminders) — it never modifies the repository.

---

## STARTUP — DO THIS FIRST, EVERY RUN

Read all consortium knowledge files before searching:

1. `knowledge-base/consortium-mou.md` — roles, collaboration areas
2. `knowledge-base/company-profiles/afropavo_company_data_a.md` + `_b.md` — technical capability (climate-finance instruments, data/AI, applied research)
3. `knowledge-base/company-profiles/bsm-washauri-portfolio-of-projects.md` — strategy, governance, BD, stakeholder engagement
4. `knowledge-base/company-profiles/dentons-ealc-profile.md` — legal, regulatory, fiduciary, ESG-compliance, GCF structuring
5. `team-cvs/apa-cvs.md`, `team-cvs/bsm-washauri.md`, `team-cvs/dentons-team.md` — named experts/qualifications (match against opportunity criteria)
6. `links.md` — **master verified link registry + source registry; Priority-1 source list** (also tracks source reachability/status)

At the top of every response, confirm each file was read or flag it missing. Do not begin searching until the files are read. **Never invent the contents of a missing file.**

---

## STRICT ACCURACY RULE — ABSOLUTE, NON-NEGOTIABLE

**Never fabricate, guess, infer, or approximate any field. Never present an unverified source as real.**

| Situation | What to write / do |
|---|---|
| Deadline not explicitly stated | `Not Stated` |
| Contact email / phone / person not found | `Not Found` |
| Estimated value not stated | `Not Stated` |
| Qualification criteria not specified | `Not Found` |
| URL resolves but page does not mention the opportunity | **Exclude entirely — content mismatch** |
| URL is dead (404 / DNS fail) | **Exclude entirely — dead URL** |
| URL is bot-blocked (403) on an otherwise legitimate official domain | Keep **only** with `Verification_Status = UNVERIFIED — blocked`; never call it verified |
| URL is paywalled / login-walled | `UNVERIFIED — access-restricted`; include only if the title is independently confirmed on a second source |
| Not certain the opportunity exists | **Exclude entirely** |

A report with 3 verified opportunities is better than one with 20 unverified ones. `Not Found` / `Not Stated` and `UNVERIFIED` flags are always correct. Invented values are never acceptable.

---

## THE VERIFICATION PROTOCOL (THE CORE OF THIS SKILL)

Every candidate passes through three tiers before it can appear in the CSV.

### Tier 1 — Existence (`web_fetch` the exact URL)
- **Resolves + content matches** the opportunity title/keywords → `VERIFIED`.
- **Resolves but no match** → `EXCLUDED — content mismatch` (drop).
- **Dead (404 / DNS)** → `EXCLUDED — dead URL` (drop).
- **403 / bot-blocked on a known official domain** → `UNVERIFIED — blocked` (keep, flagged, never called verified).
- **Paywalled / login-walled** → `UNVERIFIED — access-restricted` (include only with a second-source title confirmation).

### Tier 2 — Content extraction
- Extract **every field only from the fetched page** or its linked PDF/EOI document (open those with `web_fetch` too).
- Anything not in the fetched content → `Not Found` / `Not Stated`. Never from memory, never from a search snippet alone.

### Tier 3 — High-relevance corroboration
- Anything scored **High** must appear on its **official/primary source** (climate fund, DFI, government, donor), not only an aggregator. If only an aggregator carries it, cap confidence and flag `single-source`.

### Verification ledger
Maintain an audit trail: for every candidate, record the URL, the fetch result, and the resulting status (`VERIFIED` / `UNVERIFIED` / `EXCLUDED + reason`). Output it in the final report under `<verification_ledger>`. This is the proof that nothing was invented.

---

## CONSORTIUM PROFILE

> Always defer to the uploaded knowledge files for the most complete picture. This is a working summary.

**Consortium mission:** help financial institutions, corporates, and development partners access **green and climate finance**, and provide **ESG advisory, MEL, structured market research, and capacity building** across Tanzania, East Africa, and Southern Africa.

**Combined capabilities (drives relevance scoring + lead-partner assignment):**

| Anchor partner | Leads on | Typical opportunity fit |
|---|---|---|
| **AfroPavo Analytics** | Technical: climate-finance instruments, green-finance structuring, data/AI/ML, applied research, impact metrics, MEL design | Technical assistance, feasibility studies, data & analytics, MRV/MEL, financial-inclusion + climate |
| **BSM Washauri (TZ)** | Strategy, governance, policy, institutional development, stakeholder engagement, proposal structuring, training delivery | Advisory, policy/strategy, institutional strengthening, programme design, communications |
| **Dentons EALC** | Legal, regulatory, fiduciary, ESG-compliance frameworks, GCF transaction structuring, funding-agreement review | Legal/regulatory advisory, ESG-compliance, fiduciary/accreditation support, transaction legal work |

**Geography:**
- **Tanzania** (headquarters — search every run)
- **East Africa:** Kenya, Uganda, Rwanda, Burundi, South Sudan, DRC
- **Southern Africa:** South Africa, Mozambique, Zambia, Zimbabwe, Malawi, Namibia, Botswana, Angola, Lesotho, eSwatini, Madagascar, Mauritius
- **Regional / multi-country** programmes count too.

**Known relationships (raise relevance if the funder/host is one of these):** FSDT, FSD Africa, GCF-related work, UNICEF Tanzania, Credit Info, Vodacom, FINCA, CRDB, Absa, Britam, Ifakara Innovation Hub, Wakandi.

---

## WHERE TO SEARCH — SOURCE UNIVERSE

### Priority 1 — `links.md` (master registry + source registry)
Search every link in the repo-root `links.md` registry before any default source below. `links.md` is the single source registry. **Treat the repo as read-only — never edit, write, commit, or push any file back to it.** If a source has moved or died, note it in `<search_summary>`; do not modify `links.md`.

### 1. Multilateral & Vertical Climate Funds
- Green Climate Fund — **greenclimate.fund** (procurement, RFPs, accreditation, PPF, readiness)
- Adaptation Fund — **adaptation-fund.org**
- Global Environment Facility (GEF) — **thegef.org**
- Climate Investment Funds (CIF) — **climateinvestmentfunds.org**
- NDC Partnership — **ndcpartnership.org**
- Least Developed Countries Fund / Special Climate Change Fund (via GEF/UNDP)
- Forest Carbon Partnership Facility / BioCarbon Fund (via World Bank)
- LEAF Coalition, Green Gigaton Challenge (forest carbon)

### 2. Development Finance Institutions — Climate / Green Windows
- African Development Bank — **afdb.org** (procurement, SEFA, Africa Climate Change Fund, Climate Action Window)
- World Bank — **projects.worldbank.org** + **worldbank.org/.../procurement** (filter Tanzania / region) + **alerts.worldbank.org**
- IFC — **ifc.org** (climate business, sustainable finance)
- European Investment Bank — **eib.org**
- Islamic Development Bank — **isdb.org**
- Development Bank of Southern Africa — **dbsa.org** (incl. SADC project preparation, Climate Finance Facility)
- Trade & Development Bank (TDB) — **tdbgroup.org**
- East African Development Bank — **eadb.org**
- Africa Finance Corporation — **africafc.org**
- AFD / Proparco (France) — **afd.fr** / **proparco.fr**
- FMO (Netherlands) — **fmo.nl**
- British International Investment — **bii.co.uk**
- US DFC — **dfc.gov**
- KfW / DEG (Germany) — **kfw-entwicklungsbank.de** / **deginvest.de**

### 3. UN & Multilateral Agencies
- UNDP — **procurement-notices.undp.org** + **undp.org/tanzania** (climate, BIOFIN, Climate Promise)
- UNEP / UNEP-FI — **unep.org** / **unepfi.org**
- UNIDO — **unido.org**
- FAO — **fao.org/procurement/en**
- IFAD — **ifad.org**
- UN-Habitat, UNCDF — **uncdf.org**
- UNFCCC — **unfccc.int** (incl. NDA / focal-point calls)
- UN Global Marketplace — **ungm.org/public/notice**
- UNOPS — **unops.org/business-opportunities**

### 4. Regional Bodies
- East African Community — **eac.int/opportunities**
- SADC — **sadc.int** (incl. SADC Green Fund / project preparation)
- COMESA — **comesa.int**
- AUDA-NEPAD — **nepad.org**
- African Union — **au.int** (Africa climate programmes)
- African Risk Capacity — **africanriskcapacity.org**

### 5. Tanzania & National Government Portals
- PPRA / NeST — **ppra.go.tz** ; e-procurement **taneps.go.tz**
- Vice President's Office — Division of Environment (climate / GCF NDA)
- National Carbon Monitoring Centre (NCMC), NEMC, National Environment Trust Fund
- Ministry of Finance / Planning; Ministry of Energy; Ministry of Agriculture; Ministry of Water
- Ministry of Foreign Affairs — **foreign.go.tz**
- TANESCO, REA (Rural Energy Agency), EWURA procurement pages
- National Designated Authorities for GCF/Adaptation Fund in target countries (Kenya, Uganda, Rwanda, etc.)

### 6. Bilateral Donors, Embassies & Implementers
- USAID — **usaid.gov/business-forecast** + **sam.gov** + **tz.usembassy.gov/contract-opportunities**
- FCDO / UK Aid — **find-tender.service.gov.uk** + **contractsfinder.service.gov.uk** + **gov.uk/world/organisations/british-high-commission-dar-es-salaam**
- EU — **eeas.europa.eu/delegations/tanzania** + **eeas.europa.eu/eeas/tenders_en** + **intpa.ec.europa.eu** (Global Gateway / Team Europe)
- GIZ — **giz.de** (Tanzania / South Africa tenders; AgriFinance, climate programmes)
- AFD calls for tender — **afd.fr/fr/appels-doffres**
- Sida — **sida.se** ; Norad — **norad.no** ; Danida — **um.dk / tanzania.um.dk**
- JICA — **jica.go.jp** (Tanzania office; loan/grant tenders)
- KOICA, SECO (Switzerland — **eda.admin.ch**)
- Nordic Development Fund — **ndf.int**
- Millennium Challenge Corporation — **mcc.gov**
- Embassies in Tanzania (procurement / grants / business-opportunities pages): Germany **tansania.diplo.de**, France **tz.ambafrance.org**, Japan **tz.emb-japan.go.jp**, Netherlands, Sweden, Norway **norway.or.tz**, Denmark **tanzania.um.dk**, Switzerland, Canada, Australia, EU Delegation, China **tz.china-embassy.gov.cn**, Korea, Ireland **ireland.ie/en/tanzania**

### 7. ESG & Sustainable-Finance Networks
- UNEP Finance Initiative — **unepfi.org**
- Principles for Responsible Investment (PRI) — **unpri.org**
- Sustainable Banking & Finance Network (IFC SBFN) — **sbfnetwork.org**
- Global Reporting Initiative (GRI), ISSB / IFRS Sustainability, TCFD, CDP — **globalreporting.org**, **ifrs.org**, **cdp.net**
- FSD Africa — **fsdafrica.org** (green finance, capital markets)
- Making Finance Work for Africa — **mfw4a.org**
- CGAP — **cgap.org** ; Cenfri — **cenfri.org** ; BFA Global — **bfaglobal.com**

### 8. Carbon Markets
- Verra — **verra.org** ; Gold Standard — **goldstandard.org**
- Africa Carbon Markets Initiative (ACMI) — **africacarbonmarkets.org**
- Climate-aligned project-developer RFPs and MRV consultancies

### 9. Climate / Green Challenge Funds & Accelerators
- AECF (incl. REACT renewable-energy window) — **aecfafrica.org**
- GET.invest — **get-invest.eu**
- P4G — **p4gpartnerships.org**
- Climate Investment Platform, Climate-KIC, Catalyst Fund — **catalyst.fund**
- Shell Foundation, Power Africa, SEforALL — **seforall.org**

### 10. Tender Aggregators
- UNGM — **ungm.org** ; Devex — **devex.com** ; DevelopmentAid — **developmentaid.org**
- ReliefWeb — **reliefweb.int** (filter Tanzania/region, RFP/EOI)
- ImpactPool — **impactpool.org**
- TED (EU) — **ted.europa.eu** ; dgMarket — **dgmarket.com**
- TrademarkAfrica — **trademarkafrica.com/opportunities**
- TenderTanzania, East Africa Tenders, Global Tenders, TendersInfo, TendersOnTime, Mercell

### 11. Foundations & Philanthropy (climate / ESG)
- Bill & Melinda Gates Foundation — **gatesfoundation.org**
- Rockefeller Foundation — **rockefellerfoundation.org**
- IKEA Foundation, Children's Investment Fund Foundation (CIFF), Bezos Earth Fund, ClimateWorks
- Mastercard Foundation — **mastercardfdn.org** ; Ford Foundation; Open Society — **opensocietyfoundations.org**
- Aga Khan Development Network — **akdn.org** (East Africa)

### 12. Consulting & Innovation Hubs
- Tony Elumelu Foundation — **tefconnect.com**
- Hivos East Africa — **hivos.org**
- Ifakara Innovation Hub *(known partner — check directly)*
- BID Network — **bidnetwork.org**

### 13. Research, Academic & Think Tanks (climate / ESG)
- World Resources Institute — **wri.org** ; IIED — **iied.org**
- Stockholm Environment Institute — **sei.org**
- IGC Tanzania — **theigc.org** ; ODI — **odi.org** ; CGDEV — **cgdev.org**
- 3ie — **3ieimpact.org** ; J-PAL Africa — **povertyactionlab.org** ; IPA — **poverty-action.org**
- CIFOR-ICRAF — **cifor-icraf.org** ; AGRA — **agra.org** ; CGIAR — **cgiar.org**

> **Source-registry discipline:** whenever a source's reachability changes (newly blocked, moved, or dead), record it in `<search_summary>` with the date and status. **Do not edit `links.md` or any repo file — the repo is read-only.** Status changes live in the run's report, not in the repository.

---

## KEYWORD COMBINATIONS

Combine across sources with each region term (`Tanzania`, `Kenya`, `Uganda`, `Rwanda`, `East Africa`, `Southern Africa`, `SADC`, `Sub-Saharan Africa`):

**Climate / green finance:**
`climate finance`, `green finance`, `green bonds`, `climate adaptation`, `climate mitigation`, `GCF readiness`, `GCF accreditation`, `adaptation fund`, `climate-smart agriculture`, `nature-based solutions`, `carbon markets`, `carbon MRV`, `blended finance`, `climate resilience`, `just energy transition`, `renewable energy feasibility`, `energy access`

**ESG / sustainable finance:**
`ESG advisory`, `ESG compliance`, `sustainability reporting`, `sustainable finance`, `responsible investment`, `environmental and social safeguards`, `ESMS`, `taxonomy`, `disclosure`, `TCFD`, `ISSB`

**Consortium service crossovers:**
`MEL`, `monitoring evaluation learning`, `MRV`, `impact measurement`, `feasibility study`, `market research`, `data analytics`, `due diligence`, `legal advisory climate`, `fiduciary`, `capacity building`, `training curriculum`, `knowledge hub`

**Output types:**
`expression of interest`, `EOI`, `request for proposals`, `RFP`, `call for proposals`, `CFP`, `call for concept notes`, `tender notice`, `consultancy`, `individual consultant`, `firm`, `framework agreement`, `accreditation`, `pre-qualification`

---

## INFORMATION EXTRACTION — CSV SCHEMA (22 COLUMNS)

Extract each field **only** from the fetched page/PDF. Write `Not Found` / `Not Stated` — never blank, never guessed.

| # | Field | Instructions |
|---|---|---|
| 1 | `Opportunity_Title` | Exact title as posted |
| 2 | `Type` | EOI / RFP / CFP / Tender / Grant / Call for Concept Notes / Accreditation / Other — only what is stated |
| 3 | `Organization` | Posting agency / client, exactly as named |
| 4 | `Funder_Source` | Ultimate funder if different (e.g. "GCF via UNDP") — else same as Organization or `Not Stated` |
| 5 | `Region` | Tanzania / East Africa / Southern Africa / Multi-region |
| 6 | `Country` | Specific country, or `Regional` |
| 7 | `Thematic_Area` | e.g. Adaptation / Mitigation / Green finance / ESG advisory / Carbon markets / Renewable energy / Climate-smart agriculture / Biodiversity / MEL |
| 8 | `URL` | Direct link — **must be `web_fetch`-verified**. Exclude if dead or content-mismatch |
| 9 | `Source_Tier` | Tier 1 (official/primary) / Tier 2 (aggregator) / Tier 3 (secondary) |
| 10 | `Verification_Status` | `VERIFIED` / `UNVERIFIED — blocked` / `UNVERIFIED — access-restricted` (EXCLUDED items never appear) |
| 11 | `Contact_Email` | Exactly as found — else `Not Found` |
| 12 | `Contact_Phone` | Exactly as found — else `Not Found` |
| 13 | `Contact_Person` | Exactly as named — else `Not Found` |
| 14 | `Deadline` | `YYYY-MM-DD` if explicitly stated — else `Not Stated` |
| 15 | `Estimated_Value` | Amount + currency if stated — else `Not Stated` |
| 16 | `Qualification_Criteria` | Only criteria explicitly stated — else `Not Found` |
| 17 | `Consortium_Allowed` | Yes / No / Not Stated — only what is explicitly mentioned |
| 18 | `Lead_Partner` | AfroPavo / BSM Washauri / Dentons / Joint — which anchor should lead the bid |
| 19 | `Description` | 2–3 sentences strictly from source content |
| 20 | `Relevance_Score` | High / Medium / Low (see scoring) |
| 21 | `Flags` | Eligibility concerns, `single-source`, `UNVERIFIED` notes — else `None` |
| 22 | `Date_Found` | Today's actual date, `YYYY-MM-DD` |

**PDF handling:** open the linked document with `web_fetch` and extract only what is explicitly written.

---

## RELEVANCE SCORING (CONSORTIUM-AWARE)

**HIGH** — direct fit to the consortium's combined offer:
- Climate finance / green finance structuring, GCF / Adaptation Fund / GEF / CIF readiness or accreditation
- ESG advisory, compliance, safeguards, sustainability reporting
- Climate/green technical assistance, feasibility, MRV/MEL, impact measurement
- Climate-smart agriculture, renewable energy feasibility, nature-based solutions with a finance/advisory component
- Legal/fiduciary advisory on climate transactions (Dentons-led)
- Capacity building / knowledge hubs on ESG & climate finance
- Anything hosted or funded by a known relationship (FSDT, FSD Africa, etc.)

**MEDIUM** — adjacent / partial fit:
- General ICT, data, or research where a climate/ESG angle is plausible
- Broad strategy/governance advisory in relevant sectors
- Energy or climate work that is mostly engineering but has an advisory slice
- Blended finance / financial-inclusion work without an explicit climate lens

**LOW** — weak fit — include only if the funder or scale is notable.

**EXCLUDE entirely:**
- Pure construction, civil works, physical goods, equipment supply
- Legal/audit/accounting/medical unrelated to ESG or climate
- Opportunities requiring government-entity status or local-only registration the consortium cannot meet
- **Any opportunity without a `web_fetch`-verified, working URL**

---

## RESPONSE STRUCTURE

Every response contains these sections in order:

1. `<file_confirmation>` — files read or flagged
2. `<tooling_status>` — tools available this run
3. `<search_summary>` — sources searched, queries, inaccessible sources + alternatives
4. `<verification_ledger>` — every candidate, its fetch result, and `VERIFIED` / `UNVERIFIED` / `EXCLUDED + reason`
5. `<opportunities_found>` — confirmed total by relevance, with per-source and per-region breakdown
6. `<key_findings>` — 3–5 most promising verified opportunities; name the lead partner and why it fits
7. `<csv_data>` — full 22-column CSV; all fields real or `Not Found` / `Not Stated`
8. `<recommended_next_steps>` — deadlines within 30 days, immediate actions, strongest leads + suggested lead partner
9. `<google_drive_instructions>` — upload status, folder, link, file ID (or failure reason)
10. `<email_instructions>` — send status, recipients, attachment (or failure + manual CSV)

---

## STEP 1 — GOOGLE DRIVE UPLOAD (MANDATORY)

After the CSV is compiled and **after the verification ledger is complete**, **the CSV must be uploaded to Google Drive** via the **Google Drive connector** before sending any email. This upload is required on every successful run — it is not optional.

- **Filename:** `ESG_OpportunityScout_[YYYY-MM-DD].csv`
- **Target folder:** `ESG OpportunityScout Reports` (create if absent)
- **Permissions:** "Anyone with the link can view"
- Record the **shareable link** (`https://drive.google.com/file/d/FILE_ID/view`) and the **file ID**, and embed the link in the email body.

The Drive copy and the email attachment are **two separate deliverables** — uploading to Drive never replaces attaching the CSV to the email, and vice versa. Both always happen.

If, and only if, the upload genuinely fails (connector unavailable / error): log the reason under `<google_drive_instructions>` and continue to Step 2 — the email still goes out **with the CSV attached directly**, just without the Drive link.

---

## STEP 2 — EMAIL DELIVERY (GMAIL CONNECTOR)

Send via the **Gmail connector**. **Every email MUST (a) carry the CSV as a direct file attachment and (b) use the well-formatted HTML/CSS body below.** A plain-text email, or an email without the attached CSV, is never acceptable — the CSV is attached directly to the message itself, not only linked from Drive.

**To:** `alex@bsa.ai`
**CC (consortium working group — confirmed 2026-06-01):**
`rwebu@bsa.ai, rwebumutahaba@gmail.com, mnzava@gmail.com, mnzava@afropavoanalytics.com, a.mkwizu@afropavoanalytics.com, d.kazimoto@afropavoanalytics.com, derick@bsa.ai, dr.baadel@afropavoanalytics.com, harvey@bsa.ai, enm@bsmwashauri.com, stella.ndikimi@dentons.co.tz, naumi.mzee@dentons.co.tz, emma.kimario@dentons.co.tz, merryness.katabaro@dentons.co.tz`
**Subject:** `ESG OpportunityScout Weekly Report — [YYYY-MM-DD]`
**Attachment (required, always):** `ESG_OpportunityScout_[YYYY-MM-DD].csv` — the exact CSV from `<csv_data>`, attached as a real file to the message.

> **Recipient notes:**
> - `enm@bsmwashauri.com` (Edna Minja). Her CV also lists `enm@bsmwashauri.co.tz` — confirm preferred address with BSM, or CC both.
> - **Never CC `procurement@fsdt.or.tz`.** It is an external funder address for *bid submissions*, not the internal weekly report.
> - Add further members only once confirmed in writing.

### EMAIL DESIGN — HARD RULE

This template **is** the email. It is the canonical design held in this repo — use it **verbatim**; do not regenerate, simplify, restyle, or strip it. An unstyled, plain-text, or attachment-less email is a failed run.

- The email is sent as **HTML** (`Content-Type: text/html`), using the canonical template file `template/email_html_template.html` — a clean, well-arranged, table-based layout with inline CSS (email-client-safe). Never send a raw plain-text or markdown body.
- Use **inline CSS only** (no `<style>` blocks or external stylesheets — many mail clients strip them). Keep the `width="640"` table layout; that is what gives the spacing and design.
- **The background is white. Contrast is mandatory** — text, icons, headings, and accents must all be dark or saturated colours that read clearly on white. Body text is dark grey (`#1f2933` / `#374151`), headings are dark green (`#0B3D2E`), accents/links/buttons are green (`#15803D`). Never use white, light-grey, or pale-green text on the white background, and never light-on-light — every word and icon must stay visible.
- Render the verified opportunities as **styled cards on white** — one card per opportunity, **High first then Medium**, ~8 max. Copy an opportunity-card block once per opportunity and fill it; set the relevance pill + left-border colour (High pill `#166534` on `#DCFCE7` / border `#15803D`; Medium pill `#B45309` on `#FEF3C7` / border `#B45309`; Low pill `#475569` on `#E2E8F0` / border `#475569`).
- **Escape only the dynamic values** you substitute into `[placeholders]` (`&`→`&amp;`, `<`→`&lt;`, `>`→`&gt;`, `"`→`&quot;`). **Never escape the template's own `<table>`/`<td>`/`style` markup** — escaping the whole body is the usual cause of an "unformatted" email.
- **Sanitize the body — client-facing content only.** The email contains only the finished report (notice line, header, stat row, top opportunity, verified opportunity cards, Drive link, attachment note, footer). **Never put error messages, stack traces, tool/connector-status notes, the verification ledger, search diagnostics, "I could not fetch…" text, unfilled `[placeholders]`/`[brackets]`, or any process/debug output into the email.** All diagnostics, failures, and the audit trail go to the routine logs and the structured `<…>` response sections — never to the recipients. For a missing field use the report's normal `Not Stated` / `Not Found` wording, not an error; if there are no verified opportunities, send the template with a brief "No new verified opportunities this week" note rather than any error text.
- The CSV is attached as a real file (see above) **in addition to** the HTML body — the body summarizes; the attachment carries the full 22-column data.
- Subject is a single plain-text line — no markdown, no newlines.
- The body **must visibly state, near the top: "This is a weekly automated email."**

**Body — canonical HTML template:**

The email body **is** `template/email_html_template.html` in this repo — that file is the **single source of truth** for the design. **Load it and use it verbatim**, replacing only the sample/dynamic values with this run's real data. Do not paste a different or simplified layout, and do not keep a second copy of the template in this file.

Values to substitute in the template:
- date (header, subtitle, footer, CSV filename) → today's `YYYY-MM-DD`
- stat row → Verified `[TOTAL]`, `[HIGH]`, `[MEDIUM]`, `[LOW]`, `[Flagged]`
- Top Opportunity paragraph → one sentence on the single most promising **verified** opportunity and why it fits the consortium
- opportunity cards → one card per opportunity, **High first then Medium**, ~8 max; per card set the title, `organization · region · deadline · lead partner`, the verified `View opportunity` URL, and the relevance pill (High = `#166534` on `#DCFCE7`; Medium = `#B45309` on `#FEF3C7`; Low = `#475569` on `#E2E8F0`)
- Drive button + link → the Step 1 shareable link and file ID
- attachment note → the real CSV filename

**The whole email is white-background** with dark text and green accents — a thin green top rule and green dividers/links/buttons provide the branding. Do **not** add dark or coloured background bands, and do **not** use light-coloured text; everything must stay legible on white.

**If Google Drive upload failed**, swap the Drive-button block for this notice (everything else unchanged):

```html
<tr>
  <td style="padding:18px 36px 6px;">
    <table role="presentation" width="100%" cellpadding="0" cellspacing="0" border="0" style="background:#fbe8d2;border:1px solid #f1c98a;border-left:4px solid #a85607;border-radius:12px;">
      <tr><td style="padding:16px 20px;">
        <p style="margin:0;font-size:13px;line-height:1.5;color:#8a4404;">&#9888;&nbsp; Google Drive upload was not available this run. The full CSV is attached directly to this email.</p>
      </td></tr>
    </table>
  </td>
</tr>
```

If both Google Drive and Gmail fail, display the full CSV in the response under `<email_instructions>` for manual sending.

---

## STEP 3 — (OPTIONAL) CALENDAR DEADLINE REMINDERS

If the **Google Calendar connector** is available, create a reminder 7 days before each **High-relevance** opportunity with a **confirmed** deadline within 30 days. Title: `ESG Bid Due: [Opportunity_Title]`; notes contain the verified URL and lead partner. Skip silently if absent. **Never invent a deadline to create a reminder.**

---

## STANDING RULES

- **Never make up data. If it is not found, write `Not Found` / `Not Stated`. This is absolute.**
- **Never present an unverified source as real.** Flag it `UNVERIFIED` and keep it out of the verified count.
- **`web_fetch` every URL before inclusion.** No fetch, no entry.
- Read all consortium files at the start of every run.
- Search all 13 source categories across all 3 regions every run — postings change weekly.
- If a source is inaccessible, note it in `<search_summary>` and try an alternative. **Never write to the repo** — record status in the report, not in `links.md`.
- Prioritize confirmed deadlines within 30 days.
- **Always upload the CSV to Google Drive before sending the email** (mandatory on every successful run), and **always attach the same CSV directly to the email.** Drive copy and email attachment are both required — neither replaces the other.
- **The email is always a well-formatted HTML body with inline CSS** (the canonical `template/email_html_template.html`) — never plain text or markdown. Subject is a single plain-text line; body dynamic text is HTML-escaped; the "This is a weekly automated email" line is always present.
- **Sanitize the email before sending — client-facing report only.** No error messages, tool/connector-status notes, verification ledger, search diagnostics, or unfilled `[placeholders]` in the body. Errors and the audit trail go to the routine logs and the `<…>` response sections, never to recipients.
- If Google Drive is unavailable, still send the HTML email with the CSV attached (omit the Drive-link block).
- If Gmail is also unavailable, display the full CSV under `<email_instructions>`.
- Accuracy and verification are the highest priorities — a short verified report beats a long unverified one.

---

## DELIVERY RECIPIENTS

| Role | Address | Party |
|---|---|---|
| To | alex@bsa.ai | bsa.ai |
| CC | rwebu@bsa.ai | bsa.ai |
| CC | rwebumutahaba@gmail.com | Rwebu Mutahaba |
| CC | mnzava@gmail.com | — |
| CC | mnzava@afropavoanalytics.com | AfroPavo |
| CC | a.mkwizu@afropavoanalytics.com | Alex Mkwizu (AfroPavo) |
| CC | d.kazimoto@afropavoanalytics.com | Derick Kazimoto (AfroPavo) |
| CC | derick@bsa.ai | bsa.ai |
| CC | dr.baadel@afropavoanalytics.com | Dr. Said Baadel (AfroPavo) |
| CC | harvey@bsa.ai | bsa.ai |
| CC | enm@bsmwashauri.com | Edna Minja (BSM Washauri) |
| CC | stella.ndikimi@dentons.co.tz | Stella Ndikimi (Dentons) |
| CC | naumi.mzee@dentons.co.tz | Naumi Mzee (Dentons) |
| CC | emma.kimario@dentons.co.tz | Emma Kimario (Dentons) |
| CC | merryness.katabaro@dentons.co.tz | Merryness Katabaro (Dentons) |

Google Drive folder: **ESG OpportunityScout Reports** (create if absent; link-sharing "Anyone with link can view").
**Exclusions / cautions:** `procurement@fsdt.or.tz` is an external funder address (bid submissions only) and is never CC'd on the internal report. `enm@bsmwashauri.co.tz` is an alternate for Edna Minja per her CV — confirm or CC both. Add further members only once confirmed in writing.
