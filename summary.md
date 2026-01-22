### Meeting minutes (transcript summary)

#### Purpose
- Align on 2026 roadmap format: epics → deliverables, with Q1/Q2 filled first, then extend through Q3/Q4.
- Make deliverables readable for leadership without needing to open epics/stories.

---

## Key decisions / guidance
- **Deliverables are the primary artifact** (leadership reads deliverables, not epics).
- Every deliverable must include **clear scope**:
  - **In scope** (bullets)
  - **Not in scope** (bullets)
- Use **standard tags** at the start of each deliverable (e.g., **[Data Tiering]**, **[Data Products]**, **[BCBS 239]**, **[Data Authority]**, **[Data Quality]**, **[Lineage]**, **[Connectors]**) so the spreadsheet can be sorted/filtered easily.
- Keep deliverable text **functional, concise, and outcome-relevant** (remove filler like “to comply with firm-wide requirements”).
- Deliverables should represent **platform capabilities delivered by the team**, not adoption outcomes.
  - Adoption belongs to business/data-owner stakeholders (e.g., Margaret/Bala/others), not CEU/platform delivery.
- Ensure **quarter-to-quarter continuity**: Q1 deliverables should naturally lead into Q2/Q3/Q4 follow-ons (placeholders are fine).

---

## Roadmap content discussed (by theme)

### 1) Data Tiering
- Q1 deliverable: **Tier-1 report registration via templates in DataCompass + sync to Fusion** (target mentioned: end of Jan for integration).
- Known gaps to call out in deliverable scope:
  - Template limitations (approvals/reviews/manual enrichment are not systematized).
  - UI not available (current state is templates-only).
- Fusion API status:
  - Mostly works, but **some APIs are failing / small blocker** being worked with Kevin/Saurab.

### 2) Data Products / Datasets publishing to Fusion
- Clarify deliverable as **platform enablement**:
  - Capability to register data products/datasets and sync to Fusion (not “all datasets created/validated”).
- Explicitly separate:
  - Platform delivery (this team)
  - Data owner validation + adoption (other owners)
- Call out readiness details already done:
  - Portfolio / DataSpace mapping to Fusion, registration under correct DataSpaces, etc.
- Add follow-on placeholders across Q2–Q4 (especially if Fusion roadmap dependencies exist like domain hierarchy).

### 3) BCBS 239 items
- Keep BCBS items as their own tagged deliverables; don’t over-explain benefit.
- Concern: Fusion may not support **technical lineage** (Fusion seems focused on functional lineage); this is an open issue to raise/clarify with Fusion.

### 4) “Mark-to-Market” / Market risk tracking
- Clarified: **Dashboard is not the deliverable**.
- Deliverable should be: **DataCompass/Atlan data product enabling the dashboard**.
- Current implementation described:
  - Metric service data lands in **Databricks tables** (under RCD), with a view used for the dashboard.
- Requested addition:
  - Create a **DataCompass metadata/data product** (model/data dictionary) in Atlan taxonomy, linked to Databricks assets.

### 5) Connectors: Snowflake + Tableau (+ future Glue / S3 etc.)
- Current state: **not ready yet**; dependencies on Atlan connector changes.
- Strong direction:
  - Do not commit milestones that are blocked by external delivery dates.
  - Track dependencies explicitly; commit dates only after external prerequisites are truly delivered/validated.
- Strategic push:
  - It’s not acceptable to “enable 8 connectors across all of 2026.”
  - Target should be more aggressive (e.g., “by Q2”) and push Atlan accordingly.
- Action mentioned: review Atlan tracker when Amit is back; send escalation to Atlan leadership (Marianne) that phased approach won’t work.

### 6) Knowledge graph
- Explicitly **kept out of deliverables for now** (not ready / unclear scope).

### 7) Data Contracts (major focus)
- Current Q2 “enablement” wording is too vague.
- Proposed structure: **split into 3 deliverables across Q2/Q3/Q4** with dependencies called out:
  1) **Phase 1 (Q2):** initial implementation based on draft/interpretation (dependent on “2/3/5” inputs referenced)
  2) **Phase 2 (Q3):** align with firmware tooling / evolving requirements
  3) **Phase 3 (Q4):** align to published firm-wide standard procedure (expected around end of Sep)
- Concern: narrative must explain why start dates exist before firmware publishes procedures; Bala must provide a firm spec earlier rather than waiting.

### 8) Data Authority & Data Quality (UI gap)
- Repeated theme: **“You don’t support it if there’s no UI.”**
- Current support is largely **templates/APIs**, but deliverables must explicitly include **UI authoring/editing capabilities** where required (data authority designations, DQ metric authoring/editing, linking issues to metrics, etc.).
- Separate buckets:
  - BCBS DQ controls
  - DQCS integration
  - General Data Quality / DGLC requirements
  - Lineage (keep BCBS lineage separate from firmware lineage)

### 9) Out-of-year / mis-timed items
- A December “metadata enrichment” style item looked like a **2027** concern (pilot ends Dec 2026); recommendation: **don’t commit in 2026** unless required.

---

## Risks / gaps identified
- Deliverables currently **read like “done”** when critical pieces (especially UI + approvals workflows) are missing.
- Missing clear **in-scope / out-of-scope** bullets per deliverable.
- Over-reliance on external dependencies (Atlan/Fusion) without hard gating and escalation.
- Several areas need clearer separation: platform delivery vs adoption vs downstream dashboards.

---

## Action items
1) **Rewrite deliverables** with:
   - Tag prefix `[ ... ]`
   - Clear “In scope / Not in scope”
   - Platform-capability wording (not adoption wording)
2) **Add UI deliverables** where applicable (Data Authority, Data Quality, authoring/editing, linking, etc.).
3) Build **Q3/Q4 placeholder deliverables** for each tag to show year-long narrative continuity.
4) For **Data Contracts**: create **3 phased deliverables** (Q2/Q3/Q4) and document dependencies (“2/3/5”).
5) Review/refresh **Atlan dependency tracker**; prepare escalation messaging (Marianne) pushing for faster multi-connector readiness.
6) Confirm with Bala the **Data Contracts timeline rationale** and required specs; align before publishing roadmap.
7) Produce a cleaned spreadsheet version and send offline for review; keep Q1/Q2 dates realistic (avoid over-committing).
sd\\a