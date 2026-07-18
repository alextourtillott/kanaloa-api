# End-to-End Lifecyle Process 

To scale documentation alongside a rapidly evolving app like Kanaloa, content updates cannot be an afterthought. They must be deeply woven into the engineering development lifecycle.

Here is how a single code or feature change travels from a developer's machine into our live, validated user-facing documentation site.

Plaintext
[ 1. TRIGGER ] ───► [ 2. GATHER ] ───► [ 3. AI PIPELINE ] ───► [ 4. HUMAN GATE ] ───► [ 5. CI/CD VALIDATE ] ───► [ 6. PUBLISH ]
  Feature PR          Raw Notes &          Stage 1: Draft        Technical Writer       Markdown Syntax         Vercel/Netlify
  or Jira Alert       OpenAPI Diff         Stage 2: Refine       Code & UI Testing      & OpenAPI Lint          Live Build

Step-by-Step Execution Workflow
1. Triggering & Information Gathering
The documentation workflow is kicked off by one of two operational triggers:
The Code Trigger: A developer opens a Pull Request (PR) in the primary application repository that modifies files in the /api/v1/ directory or introduces changes to the user interface schema.
The Product Trigger: A feature ticket is moved to the "Ready for Docs" column in Jira or GitHub Projects.
Once triggered, a GitHub Action automatically harvests context by extracting the PR description, reviewing structural changes in the openapi.yaml file, and aggregating raw developer implementation notes associated with the branch.

2. File Creation, Refinement, & Evolution
Once context is collected, the pipeline handles file tracking asynchronously:
For New Features: The Stage 1 AI Agent identifies the target documentation directory based on the semantic shape of the feature. It auto-generates a brand new file (e.g., docs/tutorials/new-feature.md), scaffolding it with the precise Markdown templates defined in the information architecture.
For Feature Updates: If a file already exists, a diff engine passes the current version along with the new engineering updates to the AI workspace, allowing the model to surgically rewrite outdated steps or append new parameters to existing layout files.
Polishing: The text is passed directly to the Stage 2 Refiner to ensure it aligns seamlessly with Kanaloa's friendly, concise corporate voice before it hits human review.

3. Automated Validation & Quality Gates
Before any document is merged into production, it must successfully navigate two strict quality gates.
Gate A: Automated Code Integrity Linting (CI/CD)
The branch undergoes automated structural checks via GitHub Actions. If a check fails, the pipeline locks, preventing code merges until it's fixed.
Syntax Check: Verifies that all markdown layout elements are properly balanced, anchors work, and no code blocks are left open.
Drift Check: Assures that any code block samples in the documentation precisely match the endpoints declared in our openapi.yaml manifest.
Gate B: The Human Editorial Gate
A Technical Writer reviews the generated PR. They cross-verify the terminology with the live web application interface, execute the embedded code strings locally to ensure zero syntax errors, and confirm that no AI-hallucinated features slip through the cracks.

## Definition of Done (DoD)
A documentation task is only considered Done and ready for production deployment when it meets the following strict quality criteria:
Accuracy: All UI navigation directions, button selections, and feature names match the production application screens with absolute precision.
Executable Snippets: Every single code sample (Python, JS, cURL) in the file must return a successful 2xx series HTTP status response when run against the test sandbox environment.
Scannability: The text must pass automated readability filters—meaning zero paragraphs exceed 3 lines, clear bolding emphasizes core actions, and it utilizes task-oriented syntax headers.
Green CI Checks: The markdown linter and structural integration tests pass with zero warnings or errors.
Dual Sign-Off: The pull request possesses approved structural check reviews from both a Technical Writer (for style and clarity) and the lead Feature Engineer (for complete technical accuracy).
Once the Definition of Done is met, merging the branch to main automatically fires a webhook that builds and distributes the fresh documentation out to our live web portal.