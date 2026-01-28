# Meeting Minutes – Snowflake Query Mining, Redaction & Access Controls

## Purpose
Discuss how Snowflake query history and related metadata can be accessed, redacted, and used safely within JPMorgan infrastructure to support lineage, usage, and governance use cases—without exposing sensitive data.

---

## Key Discussions

### 1. Redaction & Data Security
- All extraction and processing run **entirely within JPMC infrastructure**.
- Query text is **sanitized/redacted** (sensitive literals such as names or tax IDs are removed or consistently replaced).
- No productive queries leave JPMC; only sanitized data is processed.
- Redaction does **not impact lineage**, as lineage relies on tables, columns, joins—not WHERE-clause literals.

### 2. Use of Query History
- Primary use cases:
  - **Lineage derivation**: infer table-to-table data movement by parsing SQL query text.
  - **Popularity & usage metrics**: frequency of table usage and distinct users.
- Snowflake lacks native object-level lineage; lineage must be **derived by parsing SQL**.
- Lineage is stitched end-to-end across Snowflake, BI tools, and upstream ETL systems.

### 3. Query History vs. Access History
- Query history alone is **insufficient and inefficient** for scoping to specific databases/schemas.
- **Access History** is needed to:
  - Efficiently identify which objects are accessed.
  - Avoid expensive full scans of query history.
- Direct access to raw account usage tables is **not permitted**.
- Proposed solution: provide **curated/sanitized views** over access history instead of raw tables.

### 4. Volume & Performance Considerations
- Environment has **3,000+ active users daily** and very high query volumes.
- Data extraction must be **incremental** using timestamp markers.
- Warehouses must be **properly sized** based on:
  - Objects queried
  - Frequency of extraction
  - Data volume
- Initial scope may be limited to specific databases (e.g., CCB Risk / Finance).

### 5. Clarification on “Use Cases”
- Platform team requires **clear justification** for access:
  - What metrics are required (e.g., top users, auditability, access reporting)?
  - Which Snowflake objects are needed to support those metrics?
- Focus is on **bank-relevant needs**, not all product capabilities.
- Example use cases discussed:
  - Top users of tables over the last 30 days
  - Audit reports (who accessed what)
  - Lineage visibility for governance

### 6. Governance & Operating Model
- All activity must run under the **Data Compass SEAL** with its own warehouse and billing.
- Platform team needs:
  - A prioritized list of use cases
  - Mapping of each use case to required Snowflake objects
  - Expected query patterns and approximate volumes
- Access history exposure will likely be through **custom views**, not raw tables.

---

## Action Items

### Atlan / Integration Team
- Provide a **prioritized list of concrete use cases**.
- Identify **required Snowflake objects** per use case.
- Share expected **query patterns and relative volumes**.

### Snowflake Platform Team
- Design **sanitized/curated views** over access history.
- Evaluate **warehouse sizing** once requirements are clarified.

### Follow-Up
- Schedule a **follow-up meeting** later in the week or early next week to finalize scope and access approach.

---

## Outcome
Query redaction and sanitized query history are aligned. Access history is likely required for performance and accurate scoping, but must be exposed through controlled, sanitized views. Clear, bank-defined use cases are required before broader access can be approved.
