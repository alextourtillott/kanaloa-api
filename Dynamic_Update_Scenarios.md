# Dynamic Update Scenarios

**Example 1:** Minor UI Rename / Terminology Change
The Drift Event: Engineering renames the button "Generate Report" to "Compile Ecosystem Insights" to match a marketing rebrand, and changes the API payload key water_temp_celsius to sea_surface_temp_c.
What Changes: A single UI element name and one API variable key.
Which Docs Are Hit:
docs/tutorials/create-data-report.md (Step 2 & Step 5 text updates)
docs/api/reference.md (Request parameters table and code snippets)
docs/quick-start.md (Code snippets update)
Which AI Steps Run:
Trigger: Automated regex sweep triggers when openapi.yaml changes or a code commit edits the UI button label string.
Stage 1 (Drafter): Bypassed for the tutorial doc, but runs on the API reference to update the schema parameter table and swap the key out in the Python/JS/cURL blocks.
Stage 2 (Refiner): Scans the tutorial file, swaps out the button string references, and verifies the surrounding sentence flow still reads smoothly.
How It Gets Approved: Since this is a low-risk, cosmetic change, it passes a fast-track approval gate. The CI/CD pipeline verifies that the string sea_surface_temp_c accurately maps to the OpenAPI spec. The Technical Writer does a quick visual diff check in the GitHub PR and merges it.

**Example 2:** Major Feature Launch / Workflow Restructure
The Drift Event: Kanaloa launches an entirely new automated drone mission safety layout. Instead of just configuring a single AUV sync path, users must now create an "Operations Plan" first, pass a "Pre-Dive Safety Check," and then link their drones.
What Changes: The linear configuration path is completely broken and expanded into a multi-step nested process.
Which Docs Are Hit:
docs/tutorials/configure-auv-sync.md (Complete architectural rewrite)
docs/changelog.md (Chronological release entry added)
docs/index.md (Welcome directory updated to reference the new structural term)
Which AI Steps Run:
Trigger: Product manager moves the feature epic to "Ready for Docs" in Jira, appending engineering specs and an export of the new user flow diagrams.
Stage 1 (Drafter): Consumes the raw workflow notes. It completely scraps the old sequential steps in configure-auv-sync.md and generates a fresh <Sequence> block containing three new steps matching the structural changes. It leaves an explicit comment: <!-- HUMAN REVIEW: Verify pre-dive telemetry bounds -->.
Stage 2 (Refiner): Re-evaluates the technical voice of the new draft, ensuring it provides supportive, clear instructions for engineers dealing with the new safety checks.
How It Gets Approved: High-risk gate. The Technical Writer physically logs into the staging environment and steps through the entire "Pre-Dive Safety Check" to verify the instructions match the real software behavior. The Lead Robotics Engineer must provide a peer review sign-off on the PR to guarantee safety metrics are technically correct before it goes live.

**Example 3:** Outdated or Conflicting Data Cleanup
The Drift Event: A legacy section in docs/faq.md explicitly states that Kanaloa does not support offline file caching. However, engineering just quietly merged a patch enabling offline caching for AUV logs when research vessels lose satellite connectivity. The documentation is now actively lying to the user.
What Changes: A direct logical contradiction between the application features and the static docs.
Which Docs Are Hit:
docs/faq.md (Inverted truth logic)
docs/changelog.md (New capability logged)
Which AI Steps Run:
Trigger: A nightly repository-wide semantic analysis cron job scans the entire documentation folder against the codebase's feature flag manifest files.
Stage 1 (Drafter): Identifies the specific conflicting paragraph in faq.md ("Does Kanaloa work offline? No..."). It rewrites the answer to reflect the new truth, utilizing the context grabbed from the engineering PR that closed the issue.
Stage 2 (Refiner): Polishes the rewritten FAQ entry, framing the new capability enthusiastically to match the "What's New" momentum voice.
How It Gets Approved: Standard gate. The Technical Writer runs the automated markdown linter to ensure formatting is sound. They verify that the updated FAQ matches the official product release notes, clear any pending human-review flags, and merge the patch.