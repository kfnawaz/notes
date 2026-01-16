Scrum Meeting Minutes Summary
Attendance / Context

Sam is out of office (was out yesterday too).

Scrum ran ~9:15–9:28.

Individual Updates
Sindhuri

Scope: Report model + Fusion sync + DQ Matrix API

Progress:

Report/model changes completed; PR raised and app changes in progress.

Working with Bharath to align model updates needed for Fusion sync (may still evolve).

Made changes for DQ Matrix API, assessing downstream impact.

Next:

Set up a coordination meeting with Pallavi + Sweetie + Bharath + Emmanuel to ensure DQ Matrix API changes don’t break DQ Rules work.

Other: Mentioned related story 1140 is also done and similar in scope.

Emmanuel

ATLAN-1618 (Model change):

Completed and tested code changes.

Found duplicate key / unique index conflicts during Fusion sync testing.

Implemented logic to update existing records instead of failing on DB duplicate key error.

Next: Review approach with Bharath in the 11:00 AM meeting.

ATLAN-1194 (Tier-1 report registration template):

Drafted template using an example shared by Cindhuri.

Next: Review with Bharath + Sindhuri in the same meeting.

Status: Functionally done; pending Epic Lead review/approval.

Stephen

ATLAN-1088 (kubectl remoting / k3s access):

Blocked due to k3s installation failure triggered during attempts to enable remoting.

Troubleshooting done with Melissa; limited help available from vendor.

Tried reinstall via Jules pipeline: pipeline reported success but did not actually restore binaries/configs.

Next:

Inspect Jules logs to locate failure.

Work with Nawaz + Rick and involve SRE to complete missing steps (service file, etc.).

Noted limited availability today due to an eye appointment, but will continue work.

Tableau + Snowflake connectors (vendor dependency):

Met vendor yesterday; clarified auth requirements.

Vendor committed delivery by January 30 for token-based auth mechanism.

Pilot consumers (CCB) have confirmed scope of assets to crawl for Tableau/Snowflake.

Implementation progress is blocked until vendor delivers updated token-based auth.

Auth approach details:

Tokens based on ID Anywhere.

Connector will expect token as a Kubernetes secret.

A separate process will be needed to generate/refresh token and update secrets.

Stephen pointed Avinash to a library/module (via GSM) that supports ID Anywhere and offered to help.

Decisions / Agreements

No compensation numbers are to be discussed on public calls (explicit reminder issued).

Plan to collaborate more starting Monday once India holiday ends and Nishant is expected back.

Blockers / Risks

ATLAN-1088 blocked by k3s crash/failed reinstall, preventing kubectl remoting progress.

Tableau/Snowflake connector rollout blocked on vendor delivery (Jan 30) + secret/token automation work.

Next Actions

Stephen: Continue k3s recovery (Jules logs + SRE steps) with Nawaz/Rick.

Emmanuel: Review unique-index handling + template with Bharath/Sindhuri (11:00 AM).

Sindhuri: Schedule alignment meeting with Pallavi/Sweetie/Bharath/Emmanuel to validate API change impacts.

Team: Prepare for token-as-secret workflow (token generator + Kubernetes secret update process) once vendor delivers auth changes.