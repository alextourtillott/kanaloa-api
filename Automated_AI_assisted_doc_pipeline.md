# Automated AI-Assisted Documentation Pipeline

An automated, AI-assisted documentation pipeline can scale content creation while ensuring that all user-facing documentation sounds like it was written by the same experienced technical writer.
Here is an architectural design for a three-stage, AI-assisted technical writing workflow optimized for a GitHub repository setup.

The AI-Assisted Documentation Pipeline
Plaintext
[Engineering Slack/Jira/Notes] 
             │
             ▼
┌─────────────────────────┐
│  STAGE 1: THE DRAFTER   │ ◄── Enforces Page Types & Extracts Intent
└────────────┬────────────┘
             │ (Raw Markdown Draft)
             ▼
┌─────────────────────────┐
│  STAGE 2: THE REFINER   │ ◄── Enforces Style Guide, Voice, & Accessibility
└────────────┬────────────┘
             │ (Polished Markdown)
             ▼
   [ HUMAN EDITORIAL GATE ]  ◄── Verification, Code Testing, Safety Check
             │
             ▼
┌─────────────────────────┐
│  STAGE 3: THE VALIDATOR │ ◄── CI/CD Linting, Link Checking, Schema Drift
└─────────────────────────┘
Step-by-Step AI Agent Architecture
Stage 1: The Drafter (From Engineering to Markdown)
Exact Responsibility: Translates highly technical, unstructured engineering notes, Jira tickets, or code comments into structured, user-facing documentation skeletons based on the designated page type (Task Guide, Reference, Tutorial).
Inputs: Raw bullet points from developers, Slack chat logs, or a PR description.
Outputs: A structured, logically ordered Markdown (.md) file utilizing proper headers (##, ###) and task-oriented language.
Risk Mitigation: Specifically targets invented engineering context. Developers often write shorthand that implies a step; this model expands that into chronological order to catch missing prerequisites.
Repository File: .github/ai-prompts/stage1-drafter.txt
Plaintext
SYSTEM INSTRUCTIONS: You are a Lead Developer's technical assistant. Your task is to ingest unstructured engineering text and output a structured Markdown documentation draft.

RULES:
1. Identify the primary goal of the user: Is it a procedural how-to, a conceptual explanation, or an API reference?
2. Organize the content strictly using Markdown formatting. Use numbered lists only if the sequence order matters.
3. Replace all engineering jargon with plain-language action items (e.g., change "spin up a container" to "launch the application environment").
4. If a step mentions an action without explaining the prerequisite, insert a placeholder comment: `<!-- HUMAN REVIEW: Detail how the user reaches this screen -->`.
5. NEVER invent API URLs, code parameters, or UI element names that were not explicitly provided in the input text.
Stage 2: The Refiner (Style & Voice Enforcement)
Exact Responsibility: Ingests raw markdown text and refines it to match Kanaloa's specific organizational voice guidelines (friendly, clear, concise, scannable).
Inputs: The raw markdown output from Stage 1.
Outputs: A polished, production-ready markdown file.
Risk Mitigation: Catches technical text walls and structural drift. It ensures that no text block runs longer than three sentences and adds blockquotes or bolding to emphasize safety boundaries.
Repository File: .github/ai-prompts/stage2-refiner.txt
Plaintext
SYSTEM INSTRUCTIONS: You are Kanaloa's Senior Copy Editor. Your job is to refine technical text to make it engaging, accessible, and scannable for Marine Biologists and Robotics Engineers.

VOICE GUIDELINES:
- Tone: Empathetic, helpful, and collaborative peer. Avoid the tone of a rigid lecturer.
- Conciseness: If a sentence can be cut without losing technical weight, remove it. 
- Formatting: Ensure no paragraph exceeds 3 sentences. Use Markdown bolding (`**`) on critical UI buttons or key terms to enable scannability.

STRICT CONSTRAINT: 
Do not change code snippets, JSON objects, path names, or URLs. Your edits should focus exclusively on the surrounding descriptive and procedural text blocks.

The Human Editorial Gate (Crucial Stage)
Before any document moves to production, a technical writer must perform a manual check. AI excels at style and structure but lacks real-world contextual awareness.

MANUAL REVIEW CHECKLIST:
1. Code Validation: Copy and run the generated cURL/Python snippets in a local sandbox environment.
2. UI Verification: Manually click through the web app to ensure buttons match the text exactly.
3. Hallucination Audit: Scan for "hallucinated features"—AI often tries to be helpful by inventing intuitive UI elements (like a "Download" button) that do not actually exist in the product yet.
Stage 3: The Validator (Automated CI/CD Linting)
Exact Responsibility: Functions as a strict automated gate embedded within the GitHub Pull Request workflow. It checks formatting syntax and ensures no breaking changes are introduced.
Inputs: The final edited markdown file submitted via a Git Pull Request.
Outputs: Pass/Fail check status in the GitHub interface with a line-by-line linting report.
Risk Mitigation: Catches broken anchors, invalid markdown syntax, unescaped characters, and schema drift against the openapi.yaml file.
Repository File: .github/workflows/docs-validator.yml
YAML
name: Docs Validation Pipeline

on:
  pull_request:
    paths:
      - 'docs/**'

jobs:
  validate-docs:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Markdown Linting Check
        uses: DavidAnson/markdownlint-cli2-action@v1
        with:
          globs: 'docs/**/*.md'

      - name: Verify API Link Integrity
        run: |
          echo "Scanning docs for broken internal hooks, placeholders, and unescaped HTML characters..."
          # Uses grep to catch unresolved AI placeholders or unescaped XML brackets
          ! grep -r "<!-- HUMAN REVIEW:" docs/
          ! grep -r "YOUR_API_KEY_HERE" docs/tutorials/
Specific Hallucinations Caught by This Framework
By decoupling drafting from structural validation, this three-step workflow architecture is explicitly designed to handle typical LLM writing vulnerabilities:
The "Helpful Feature" Injection: AI models frequently invent intuitive workflows to smooth out a clunky engineering note (e.g., writing "Simply click the Undo button" when the software lacks an undo function). Stage 1 blocks this by explicitly banning unprovided UI text, and the Human Gate acts as the final check.
Syntax Breaking: LLMs often break Markdown syntax when injecting code code fences inside list loops. Stage 3 (CI/CD) automatically halts the release pipeline if code blocks are not closed properly.
Stale Configuration Snippets: If engineering changes an API payload structural parameter, an LLM might draw from its training data rather than the updated framework. The pipeline mitigates this by validating code parameters directly against the repository's native openapi.yaml file.