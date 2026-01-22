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
- Current state: **not ready yet**;
