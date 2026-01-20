# Meeting Summary – Atlan–Snowflake Connector & Metadata Access

## Purpose
Discuss onboarding and data-access requirements for integrating Atlan with Snowflake, with a focus on metadata ingestion, lineage, and governance constraints.

---

## Key Topics & Discussion

### Metadata Collection & Data Residency
- Asset discovery and lineage computation are performed by Atlan’s engine.
- Query history, access history, and raw metadata remain within the JPM platform.
- Primary concern: exposure of sensitive query predicates and joins.

### High-Risk Data Elements
**Major concerns:**
- Query History  
- Access History  

- These are not available via Information Schema and raise redaction and security concerns.
- Technology and CTC partners are especially sensitive to these datasets.

### Information Schema vs Account Usage
- Information Schema is more readily accessible and lower risk.
- Account Usage provides richer details but includes sensitive histories.
- No existing Snowflake role cleanly excludes only query/access history.

### Dependency / DST Discussion
- A Dependency (DST) may be required if new platform capabilities must be built.
- **Decision:** Do not create DST yet—wait until Atlan clarifies limitations and needs.

### Phased / Conditional Access Approach
Possible temporary access (with guardrails) if Atlan can:
- Demonstrate configurability to not consume query/access history.
- Provide proof (similar to prior DataHub precedent).

**Alternatives:**
- Platform-provided redacted views
- Future APIs

### Upcoming Platform Capability
- A new redacted query-history view is expected later this month.
- Could partially replace the need for raw query/access history.
- Retention limitation: Snowflake usage data is only retained for one year.

### Onboarding & Compute
- DataCompass must onboard as a consuming application.
- Dedicated warehouse and CLID-based billing required.
- This onboarding can proceed in parallel, independent of metadata decisions.

---

## Decisions / Agreements
- Proceed with Snowflake onboarding and warehouse setup immediately.
- Treat query history and access history as **Phase 2**, pending further clarity.
- No DST creation until requirements and gaps are fully validated.
- Explore use of the upcoming redacted view as an interim solution.

---

## Action Items

### For Nawaz / Team
- Engage Atlan to clarify:
  - Impact if query history & access history are excluded.
  - Whether exclusion is configurable and auditable.
  - Timeline implications under a phased approach.
- Share delivery timelines and priority milestones.
- Send Carol a checklist of onboarding steps already completed.

### For Platform / Carol
- Confirm availability and scope of the new redacted query-history view.
- Advise on any tactical, short-term access options with governance controls.

### For Tyler
- Act as day-to-day contact for:
  - Snowflake onboarding
  - Warehouse provisioning
  - CLID and compute tracking

---

## Next Steps
- Schedule a follow-up checkpoint (next week).
- Include Atlan in the next discussion.
- Prepare a status update for management ahead of the **31st**.

---

### Optional Follow-ups
- Convert this into a formal meeting minutes document
- Extract a problem statement + decision log
- Create a tracker/checklist for onboarding and dependencies
