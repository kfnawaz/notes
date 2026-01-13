### Topic
Atlan Snowflake connector authentication within JPMC

### Key Points
- Atlan connector supports **basic auth** and **key-pair auth**.
- **JPMC requires External OAuth (IDWorks)** for Snowflake service-to-service access.
- Current sample implementation handles **token expiry client-side**, not via Snowflake JDBC auto-refresh.
- Unclear whether **Personal Access Tokens (PATs)** are supported or permitted in JPMC.
- Need a **fully automated, policy-compliant authentication** approach.

### Decision
- Engage **JPMC Snowflake Platform** and **Application Security** teams to validate supported authentication mechanisms and access models.

### Contacts
- Snowflake Platform: **Taylor Payne**, **Karen**

### Action Items
- Send formal email (include Nawaz, Atlan, DataCompass leads).
- Schedule working session with Snowflake admins.
- Align internally with **Avinash** and **Abby Nash** on access scope and requirements.

### Status
- **Authentication is the primary blocker**; awaiting platform and security guidance.