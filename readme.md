Meeting Minutes Summary
Purpose

Align on a Snowflake authentication approach for the connector, confirm whether token refresh is required, and agree on the next steps to unblock Snowflake onboarding and UAT environment issues.

Key Discussion Points

Token validity is ~12 hours based on prior usage patterns (similar tokens used in other integrations). The team is confident token refresh is not needed for Phase 1, assuming crawler/miner jobs complete within that window.

The main engineering complexity was token refresh handling. Passing a valid token into the Snowflake driver is considered straightforward.

A concern was raised: if workloads become very large (e.g., very frequent queries / extreme scale), token refresh may be needed later. The consensus: not a near-term risk given the initial onboarding scope.

Snowflake access will likely be application-specific (unlike Databricks where broader portfolio access evolved over time). Initial onboarding (CCB Finance) is expected to be small and well within limits.

Strong emphasis on time-to-market: Snowflake onboarding has slipped multiple times (Q2 → Q3 → Q4 → now mid/late January), and stakeholders are questioning delivery reliability.

Decisions

Adopt a phased rollout

Phase 1: No refresh logic. Generate token just before job start, store it, and run crawler/miner within token validity window.

Phase 2: Add refresh/robustness only if real operational issues emerge (scale, long jobs, token expiry).

Redaction capability is already implemented

Snowflake miner redaction was reportedly ready as early as September, but rollout was blocked once the team learned the prior authentication approach would not work.

Decouple token generation from the connector

Avoid embedding token-generation compilation steps into the connector build (painful in Databricks).

Prefer the pattern used elsewhere: external job generates token and stores it in a secure location; connector reads token at runtime.

Proposed Technical Approach

Connector will pass values like:

authenticator = oauth

token = <XYZ>
into the Snowflake driver and execute queries.

Token generation and storage should be handled externally (similar to prior patterns using secret storage).

A challenge noted: current deployments aren’t on AWS, so the team needs an alternative to AWS Secrets Manager / Lambda refresh patterns (e.g., k3s/Kubernetes secrets + scheduled job/cron-like mechanism).

Blockers / Risks

UAT environment issue: k3s problems causing instability; the team discussed wiping/reinstalling as a last resort (with caution about recreating secrets).

Availability of support personnel:

Luis only Thu/Fri; back Monday.

Nishant availability unknown.

Need to rely on Tech Support ticketing process for k3s/UAT environment issues.

Action Items (Next Steps)

Kailash / team: Remove token refresh from PRD and scope; proceed with Phase 1 implementation.

Kailash + Sutorik: Provide an updated delivery date estimate by tomorrow for Phase 1 Snowflake auth.

Nawaz + team: Ensure token-generation method and “token placement location” plan is ready so the connector can retrieve and pass token to driver.

Token decoupling track: Share an internal/external proposal describing what changes are needed on each side (token generator vs connector).

UAT/k3s:

Check Tech Support ticket for context

If needed, proceed with wipe/reinstall carefully (expect secrets recreation)

Engage support via portal/email process if internal SMEs are unavailable

Outcome

The team agreed to ship Snowflake auth quickly with reduced scope (no refresh) to unblock onboarding and regain stakeholder confidence, while running a parallel track to implement a scalable, decoupled token-generation + secret distribution mechanism for future phases.