# Scrum Meeting – Condensed Minutes

**Date:** Not specified  
**Meeting Type:** Daily Scrum  
**Format:** Jira-based updates (Done / Next / Blockers)

---

## Chris

**Jira:** ATLAN-1088 – Setup `kubectl` remoting to ATLAN environments

### Done
- Met with Nawaz and Kailash to review purpose, approach, and implementation details.

### Next
- Apply configuration changes to the UAT environment.
- Begin testing once environment access is restored.

### Blockers
- `k3s` service is in a crash loop, blocking `kubectl` access.

### Actions / Mitigation
- Contact Avinash.
- Check with Peter / Forrest for any undocumented recent changes.
- Engage the SRE team to stabilize the server.

---

## Sam

**Jira:** ATLAN-1614 – NHCD Workflow Configuration (Databricks)

### Done
- Worked on NHCD workflow configuration for Databricks.
- Core configuration is nearly complete.

### Next
- Add default filter configuration:
  - Reuse the same default filter JSON from existing Databricks workflows.
- Update Confluence documentation to explicitly include this step.

### Blockers
- None.

### Notes
- Will verify Confluence edit permissions and update documentation if allowed.
- Snowflake connectivity works locally.
- Will sync with Nawaz later today to validate setup on another machine.

---

## Emmanuel

### Jira: ATLAN-1618 – Model Change

#### Done
- Updated service layer and test cases to align with recent model changes.

#### Next
- Perform end-to-end testing today.

#### Blockers
- None.

---

### Jira: ATLAN-1194 – Finalize Template for Tier-1 Report Registration

#### Done
- Began incorporating required fields shared by Bharat.

#### Next
- Update Jira comments.
- Schedule a follow-up meeting with Bharat.
- Move ticket from **Blocked** to **In Progress**.

#### Blockers
- Pending clarification and alignment with Bharat.

---

## Nawaz

**Jira:** Tableau–Snowflake Connectivity (Exploratory / Ongoing)

### Context / Progress
- Working on establishing Tableau ↔ Snowflake connectivity.
- Asked to focus on identifying which database/schema the consumer connects to, rather than coordinating directly with the platform team.

### Current Work
- Engaging with the ATLAN vendor on Snowflake authentication issues.

### Blockers / Risks
- Vendor facing challenges using OAuth with ID Anywhere (standard enterprise authentication).
- Vendor proposing alternatives such as Programmatic Access Tokens (PAT).

### Next
- Meeting scheduled tomorrow with the ATLAN vendor to:
  - Understand OAuth / ID Anywhere limitations.
  - Align on a compliant and standard connectivity approach.

---

## General Notes
- PVR meeting was scheduled to begin immediately after the scrum.
- Shavana and Cinder were not present.
