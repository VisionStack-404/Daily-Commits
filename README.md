# Daily Commit — n8n Automated Daily-Updater (Professional Guide)

Professional, step-by-step documentation for importing, configuring, testing, and operating the n8n workflow that appends a dated entry to a `Daily` file and commits it back to GitHub.

Repository structure
- n8n/
  - Github-actions-2026-08-28.sanitized.json  — exported workflow JSON (credential placeholders). Import this into n8n Editor.
- assets/
  - workflow-diagram.svg                     — simplified visual diagram (committed earlier).
  - original-workflow-diagram.svg            — detailed original node workflow diagram (this file).
- examples/
  - Daily.md                                 — sample `Daily` file for quick testing.
- .github/
  - workflows/                               — CI workflow files (branch add-samples-and-ci includes validation CI).
- README.md                                  — this file.

Overview
This n8n workflow automates a periodic update to a text file named `Daily` in a GitHub repository. The workflow reads the file, sends it to an LLM agent which appends a new dated entry (preserving all prior entries), converts the output into a file payload, and commits the updated file back to GitHub.

Tech stack
- n8n — visual automation/orchestration platform (workflow editor, credentials manager, nodes).
- GitHub API — read and write files via repository contents endpoints (used by n8n GitHub nodes).
- OpenAI (or other LLM provider) — language model used by the AI Agent node.
- YAML / JSON — workflow export format and CI configuration.
- Git & GitHub — repository hosting, commit history, branches, pull requests.

Why this is useful
- Lightweight journaling or progress log automation.
- Demonstrates safe credential handling with n8n credentials manager.
- Example of integrating LLMs into automation while preserving data integrity.

Quick clone and open
1. Clone the repository locally (optional):

   git clone https://github.com/VisionStack-404/daily-commit.git
   cd daily-commit

2. Open n8n Editor (local or n8n.cloud) and import the sanitized workflow JSON:
   - top-left menu → Import → choose `n8n/Github-actions-2026-08-28.sanitized.json`.

Prerequisites
- n8n Editor access (self-hosted or n8n.cloud).
- GitHub account and a repository you can write to (target repo for the `Daily` file).
- GitHub Personal Access Token (PAT) or OAuth app configured with repo permissions.
- OpenAI API key (or another LLM provider supported by your n8n instance).

Node-by-node description and the parameters to set
(These are the nodes in n8n; open the imported workflow and edit these fields)

1) Schedule Trigger (n8n-nodes-base.scheduleTrigger)
- Purpose: start workflow on a schedule.
- Default parameters in this workflow: interval every 3 days, triggerAtMinute 30.
- Change: set frequency to `daily` or `hourly` if required.

2) Get a file (n8n-nodes-base.github)
- Purpose: read the current contents of the `Daily` file.
- Required parameters to set:
  - Owner: GitHub username or organization (e.g., VisionStack-404).
  - Repository: target repository name.
  - File path: `Daily` (or `daily.md` or `notes/Daily.md`).
  - Credential: select your GitHub credential (PAT/OAuth) created in n8n.

3) Extract from File (n8n-nodes-base.extractFromFile)
- Purpose: parse the file response to extract raw text (used as input to the AI Agent).
- No special config normally required unless you have binary or non-text formats.

4) AI Agent (LangChain / n8n agent node)
- Purpose: receive the existing file content and return the complete updated file content with a new dated entry appended.
- Important parameters and best practices:
  - System prompt / instructions: keep the system prompt explicit and prescriptive. Example (already included in the sanitized JSON):
    - Preserve all existing entries exactly.
    - Append one new entry for today's date at the end.
    - Do not invent projects/achievements. Do not create duplicate entries for the same date.
    - Output only the complete file content (no explanations or markup wrappers).
  - Model selection: the sanitized workflow references `gpt-5-mini` (or `gpt-4o` / `gpt-4`). Choose a model available under your OpenAI plan.
  - Temperature / creativity: set temperature low (e.g., 0.0–0.3) to reduce hallucinations and keep entries factual and concise.
  - Max tokens / output length: ensure the model can output the entire file. For large files, either reduce file size or move to dated files per entry.
  - Safety: do not pass secrets or credentials into the prompt. Keep system instructions deterministic.

5) Convert to File (n8n-nodes-base.convertToFile)
- Purpose: convert the AI Agent output to a binary/file payload ready for the GitHub edit node.
- Source property: typically `output` or the property where AI Agent placed the content.

6) Edit a file (n8n-nodes-base.github)
- Purpose: commit the updated `Daily` file back into the target repo.
- Parameters:
  - Owner / Repository / File path: must match the Get a file node settings.
  - Commit message: e.g., `docs: update Daily (automated)` — include a timestamp if needed.
  - Credential: pick the GitHub PAT/OAuth credential.
- Notes: if branch protections prevent direct commits, configure the node to create a branch and open a PR (advanced customization).

GitHub API & token generation — clean step-by-step
This section shows how to create a GitHub Personal Access Token (PAT) suitable for the GitHub nodes in n8n. Use a machine account or scoped token for automation.

1) Create a machine user (recommended)
- Optional but recommended: create a separate GitHub account (machine user) for automation so tokens are isolated from your personal account.
- Invite this machine user as a collaborator (or add to the organization) with access to the target repository.

2) Generate a Personal Access Token (PAT)
- Sign in to GitHub as the account that will own the token (machine user or your personal account).
- Navigate to: Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new token.
- Choose: **Generate new token (classic)** or the new fine-grained tokens depending on your org policy.
- Scopes to grant (minimum recommended for repo file read/write):
  - repo (Full control of private repositories) OR for fine-grained tokens, give read & write access to repository contents.
  - workflow (if you need to trigger GitHub Actions via API later).
- Name the token (e.g., n8n-daily-commit-automation) and set an expiration (30/90/365 days) as per security policy.
- Generate token and copy it immediately — you cannot see it again in the UI.

3) Add the token to n8n as a credential
- Open n8n Editor → Credentials → New → GitHub
- Choose method: Personal Access Token (PAT) and paste the token.
- Save the credential with a descriptive name (e.g., GitHub - n8n PAT).

4) Alternative: OAuth app (optional)
- For org-level integration or long-lived flows, configure a GitHub OAuth App and connect n8n via OAuth instead of PATs.
- OAuth avoids storing tokens manually but requires app registration and callback URL configuration.

n8n credential setup — detailed steps
1) GitHub credential (PAT)
- Editor → Settings → Credentials → New Credential → GitHub
- Select authentication method: Personal Access Token
- Paste the PAT and save.
- Use this credential in both Get a file and Edit a file nodes.

2) OpenAI credential
- Editor → Settings → Credentials → New Credential → OpenAI
- Paste your OpenAI API Key and save.
- Select this credential in the OpenAI Chat Model / AI Agent node.

3) Verifying credentials
- In n8n: open each node (Get a file, Edit a file, OpenAI) and choose the credential from the dropdown.
- Use the node’s test or execute function to verify connectivity (e.g., run Get a file with a known path).

Testing checklist (manual run)
- Import workflow.
- Create GitHub and OpenAI credentials in n8n and attach them to the respective nodes.
- In n8n Editor, run Execute workflow.
- Confirm logs show successful read, AI output, and GitHub commit.
- Verify commit in the target repository and the updated `Daily` file content.

Operational recommendations
- Keep `Daily` file size manageable (move older entries into archive files if it grows large).
- Prefer machine user PATs with minimum required scopes.
- Use branch+PR flow if the target repo requires code review.
- Enable execution logging and alerts in n8n if you want failure notifications.

Security and privacy best practices
- The published JSON is sanitized: credential IDs and webhook IDs were removed. Do not paste real API keys, PATs, or webhook secrets into public files.
- Use n8n credential manager and avoid storing secrets in plaintext files.
- Rotate PATs regularly and set expirations.
- For production, consider using fine-grained tokens or OAuth apps where available.

Files referenced in this repo
- n8n/Github-actions-2026-08-28.sanitized.json — import to n8n.
- assets/original-workflow-diagram.svg — detailed visual of nodes and connections.

Next actions
- I will add the remaining items into branch `add-samples-and-ci` next: examples/Daily.md (sample), CONTRIBUTING.md, LICENSE (MIT by default), .gitignore, and the GitHub Actions validate CI workflow. Confirm if you want a different license or if I should make the repository private now.