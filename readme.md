1️⃣ Formal Meeting Minutes

Topic: Atlan–Snowflake Connector Onboarding & Metadata Access
Date: (as per meeting)
Participants: Platform Team, DataCompass Team, Snowflake Platform, Atlan (to be engaged)
Prepared By: (Your name)

1. Meeting Objective

To evaluate requirements, constraints, and onboarding steps for integrating Atlan with Snowflake, focusing on metadata ingestion, lineage generation, and governance concerns related to sensitive data access.

2. Background

Atlan requires metadata (assets, lineage, dependencies) to power cataloging and governance use cases.

Snowflake metadata is available through multiple mechanisms (Information Schema, Account Usage, Query History, Access History).

Platform teams are highly protective of query predicates, joins, and access/session details due to security and regulatory concerns.

Prior attempts (2024) were limited due to incomplete onboarding and missing FIDs.

3. Key Discussion Points
3.1 Metadata Processing Model

Asset discovery and lineage computation occur within Atlan’s processing engine.

Query history and access history remain within JPM-controlled Snowflake environments.

Primary concern is exposure of sensitive query predicates and joins, not asset metadata itself.

3.2 High-Risk Data Elements

The following were flagged as high sensitivity:

Query History

Access History

These are not available via Information Schema and raise governance concerns.

3.3 Information Schema vs Account Usage

Information Schema is relatively safer and broadly accessible.

Account Usage contains richer data but introduces risk.

There is no Snowflake role that selectively excludes only query/access history.

3.4 Dependency (DST) Consideration

A Dependency (DST) may be required if new platform capabilities or views must be built.

Decision taken to delay DST creation until Atlan confirms functional limitations and absolute needs.

3.5 Phased Access Strategy

A phased onboarding approach was discussed:

Phase 1: Proceed without query/access history.

Phase 2: Revisit once redaction, APIs, or views are available.

Temporary access may be possible if Atlan can demonstrate configurability to not consume restricted data.

3.6 Upcoming Platform Capability

A new redacted query-history view is expected later this month.

This may reduce or eliminate the need for raw query/access history.

Limitation: Snowflake retains usage data for only one year.

3.7 Onboarding & Compute

DataCompass must onboard as a consuming application.

Dedicated warehouse and CLID-based billing required.

This onboarding can proceed independently of metadata access decisions.

4. Decisions

Proceed with Snowflake onboarding and warehouse setup immediately.

Treat query history and access history as Phase 2 items.

Do not create a DST until Atlan clarifies constraints and requirements.

Evaluate the upcoming redacted view as a potential interim solution.

5. Action Items

(See Tracker Section below)

6. Next Steps

Schedule a follow-up checkpoint meeting.

Include Atlan representatives in the next discussion.

Provide status update to management ahead of the 31st.