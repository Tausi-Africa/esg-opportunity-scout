---
name: esg-opportunity-scout
description: Runs a full ESG, climate-finance, and green-finance opportunity scan across Tanzania, East Africa, and Southern Africa for the AfroPavo / BSM Washauri / Dentons EALC consortium. Searches 13 source categories with targeted keyword combinations, verifies every URL and every field against live fetched pages (zero hallucination), scores consortium relevance, assigns a lead partner, compiles a 22-column CSV, archives it to Google Drive, sends a formatted HTML email via Gmail, and optionally creates calendar deadline reminders.
---

# ESG OpportunityScout — Green & Climate Finance Consortium

## What this skill does

Runs a weekly ESG / climate-finance / green-finance opportunity scan on behalf of the consortium of **AfroPavo Analytics Ltd**, **BSM Washauri (TZ) Ltd**, and **Dentons EALC** (with **CEOrt Tanzania** as institutional partner).

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

Read all files from the repository, never from a local disk. **The repo is strictly read-only — never edit, write, commit, or push anything back to it.** The link registry is **`links.md` at the repo root** (not `additional_urls.txt`); it also serves as the source registry — there is no separate `source_registry.md`. CEOrt has no dedicated profile file — describe it only from the MoU. The only outbound actions a run takes are the delivery steps (Drive upload, Gmail email, optional Calendar reminders) — it never modifies the repository.

---

## STARTUP — DO THIS FIRST, EVERY RUN

Read all consortium knowledge files before searching:

1. `knowledge-base/consortium-mou.md` — roles, collaboration areas
2. `knowledge-base/company-profiles/afropavo_company_data_a.md` + `_b.md` — technical capability (climate-finance instruments, data/AI, applied research)
3. `knowledge-base/company-profiles/bsm-washauri-portfolio-of-projects.md` — strategy, governance, BD, stakeholder engagement
4. `knowledge-base/company-profiles/dentons-ealc-profile.md` — legal, regulatory, fiduciary, ESG-compliance, GCF structuring
5. `team-cvs/apa-cvs.md`, `team-cvs/bsm-washauri.md`, `team-cvs/dentons-team.md` — named experts/qualifications (match against opportunity criteria)
6. `links.md` — **master verified link registry + source registry; Priority-1 source list** (also tracks source reachability/status)

CEOrt has no dedicated profile file — describe it only from the MoU, which names it as the knowledge-hub/training institutional partner.

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
| **CEOrt Tanzania** *(institutional)* | Knowledge hubs, learning platforms, ESG/sustainability/climate-finance training | Capacity-building, curriculum, post-training follow-up, knowledge dissemination |

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
| 18 | `Lead_Partner` | AfroPavo / BSM Washauri / Dentons / CEOrt / Joint — which anchor should lead the bid |
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
- Capacity building / knowledge hubs on ESG & climate finance (CEOrt-led)
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

**Rules:**
- The email is sent as **HTML** (`Content-Type: text/html`), using the template below — a clean, well-arranged layout with inline CSS (table-based, email-client-safe). Never send a raw plain-text or markdown body.
- Use **inline CSS only** (no `<style>` blocks or external stylesheets — many mail clients strip them). Keep the consortium green palette, the stats banner, clear section headings, and generous spacing so it reads well on desktop and mobile.
- The CSV is attached as a real file (see above) **in addition to** the HTML body — the body summarizes; the attachment carries the full 22-column data.
- Subject is a single plain-text line — no markdown, no newlines.
- All dynamic body text is HTML-escaped (`&`→`&amp;`, `<`→`&lt;`, `>`→`&gt;`, `"`→`&quot;`).
- The body **must visibly state, near the top: "This is a weekly automated email."**

**Body — HTML template (substitute all `[placeholders]`):**

```html
<!DOCTYPE html>
<html lang="en">
<head><meta charset="UTF-8"><meta name="viewport" content="width=device-width,initial-scale=1"></head>
<body style="margin:0;padding:0;background:#f4f6f9;font-family:Arial,Helvetica,sans-serif;color:#222;">
  <table width="100%" cellpadding="0" cellspacing="0" style="background:#f4f6f9;padding:24px 0;">
    <tr><td align="center">
      <table width="640" cellpadding="0" cellspacing="0"
             style="background:#ffffff;border-radius:8px;overflow:hidden;box-shadow:0 2px 8px rgba(0,0,0,.08);">

        <!-- Automated notice -->
        <tr>
          <td style="background:#0f5132;padding:8px 32px;">
            <p style="margin:0;font-size:11px;color:#d1e7dd;letter-spacing:.5px;text-transform:uppercase;">
              This is a weekly automated email
            </p>
          </td>
        </tr>

        <!-- Header -->
        <tr>
          <td style="background:#14532d;padding:26px 32px;">
            <p style="margin:0;font-size:11px;color:#9fd3b4;letter-spacing:1px;text-transform:uppercase;">
              AfroPavo &nbsp;&middot;&nbsp; BSM Washauri &nbsp;&middot;&nbsp; Dentons EALC &nbsp;&middot;&nbsp; CEOrt
            </p>
            <h1 style="margin:6px 0 0;font-size:22px;color:#ffffff;font-weight:700;">ESG OpportunityScout Weekly Report</h1>
            <p style="margin:4px 0 0;font-size:13px;color:#b7e0c6;">Green &amp; Climate Finance &middot; Tanzania, East &amp; Southern Africa &middot; [YYYY-MM-DD]</p>
          </td>
        </tr>

        <!-- Stats Banner -->
        <tr>
          <td style="background:#eaf5ee;padding:16px 32px;border-bottom:1px solid #cfe6d7;">
            <table width="100%" cellpadding="0" cellspacing="0">
              <tr>
                <td style="font-size:13px;color:#14532d;padding:4px 12px 4px 0;border-right:1px solid #c0d8c8;text-align:center;">
                  <strong style="font-size:22px;display:block;">[TOTAL]</strong>Verified
                </td>
                <td style="font-size:13px;color:#2e7d32;padding:4px 12px;border-right:1px solid #c0d8c8;text-align:center;">
                  <strong style="font-size:22px;display:block;">[HIGH]</strong>High
                </td>
                <td style="font-size:13px;color:#e65100;padding:4px 12px;border-right:1px solid #c0d8c8;text-align:center;">
                  <strong style="font-size:22px;display:block;">[MEDIUM]</strong>Medium
                </td>
                <td style="font-size:13px;color:#555;padding:4px 12px;border-right:1px solid #c0d8c8;text-align:center;">
                  <strong style="font-size:22px;display:block;">[LOW]</strong>Low
                </td>
                <td style="font-size:13px;color:#9a6700;padding:4px 12px;text-align:center;">
                  <strong style="font-size:22px;display:block;">[UNVERIFIED]</strong>Flagged
                </td>
              </tr>
            </table>
          </td>
        </tr>

        <!-- Top Opportunity -->
        <tr>
          <td style="padding:24px 32px 12px;">
            <h2 style="margin:0 0 10px;font-size:14px;font-weight:700;color:#14532d;
                        text-transform:uppercase;letter-spacing:.5px;
                        border-bottom:2px solid #14532d;padding-bottom:6px;">
              Top Opportunity This Week
            </h2>
            <p style="margin:0;font-size:14px;line-height:1.7;color:#333;">[TOP_OPPORTUNITY]</p>
          </td>
        </tr>

        <!-- Google Drive Link -->
        <tr>
          <td style="padding:8px 32px 12px;">
            <h2 style="margin:0 0 10px;font-size:14px;font-weight:700;color:#14532d;
                        text-transform:uppercase;letter-spacing:.5px;
                        border-bottom:2px solid #e0e0e0;padding-bottom:6px;">
              View &amp; Download Report
            </h2>
            <p style="margin:0;padding:14px 18px;background:#eaf5ee;border-left:4px solid #14532d;
                       border-radius:4px;font-size:13px;color:#14532d;">
              &#128193; Saved to Google Drive:<br><br>
              <a href="[GOOGLE_DRIVE_LINK]" style="color:#14532d;font-weight:700;word-break:break-all;">[GOOGLE_DRIVE_LINK]</a>
            </p>
          </td>
        </tr>

        <!-- Attachment Note -->
        <tr>
          <td style="padding:8px 32px 20px;">
            <p style="margin:0;padding:14px 18px;background:#f0f7ee;border-left:4px solid #2e7d32;
                       border-radius:4px;font-size:13px;color:#2e7d32;">
              &#128206; CSV attached: <strong>ESG_OpportunityScout_[YYYY-MM-DD].csv</strong> &mdash; every listing in it has a verified, fetched URL.
            </p>
          </td>
        </tr>

        <!-- Footer -->
        <tr>
          <td style="background:#f8f9fa;padding:16px 32px;border-top:1px solid #e0e0e0;">
            <p style="margin:0;font-size:12px;color:#888;text-align:center;">
              ESG OpportunityScout &nbsp;|&nbsp; Green &amp; Climate Finance Consortium &nbsp;|&nbsp; This is a weekly automated email &nbsp;|&nbsp;
              <a href="https://www.afropavoanalytics.com" style="color:#14532d;text-decoration:none;">afropavoanalytics.com</a>
            </p>
          </td>
        </tr>

      </table>
    </td></tr>
  </table>
</body>
</html>
```

**If Google Drive upload failed**, replace the "View & Download Report" block with:

```html
<tr>
  <td style="padding:8px 32px 12px;">
    <p style="margin:0;padding:14px 18px;background:#fff3e0;border-left:4px solid #e65100;
               border-radius:4px;font-size:13px;color:#e65100;">
      &#9888; Google Drive upload was not available this run. The full CSV is attached directly to this email.
    </p>
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
- **The email is always a well-formatted HTML body with inline CSS** (the template below) — never plain text or markdown. Subject is a single plain-text line; body dynamic text is HTML-escaped; the "This is a weekly automated email" line is always present.
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
