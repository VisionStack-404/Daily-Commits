# Daily Commit — n8n Workflow (Professional Setup Guide)

A clear, step-by-step guide to import, configure, test, and secure the n8n workflow that automatically updates a `Daily` file in a GitHub repository on a schedule.

Repository contents
- n8n/Github-actions-2026-08-28.sanitized.json — exported n8n workflow (credential placeholders).
- assets/workflow-diagram.svg — visual diagram of the workflow.
- README.md — this guide.

Overview
This workflow automates a periodic update to a text file named `Daily` in a GitHub repository. It does so by:
1. Triggering on a schedule.
2. Reading the current `Daily` file from a GitHub repo.
3. Sending the content to a language model agent to append a new daily entry.
4. Converting the agent output to a file and committing the updated file back to GitHub.

Prerequisites
- n8n (self-hosted or n8n.cloud) with Editor access.
- A GitHub account with a repository you can write to (target repo for the `Daily` file).
- A GitHub Personal Access Token (PAT) or OAuth app configured with repo permissions.
- An OpenAI API key (or another LLM provider supported by your n8n instance).

High-level steps (what you'll do)
1. Clone this repository locally (optional).
2. Import the sanitized workflow JSON into n8n Editor.
3. Create credentials inside n8n (GitHub and OpenAI).
4. Wire the credentials and target repo/file into the workflow nodes.
5. Run tests and validate behavior.
6. Activate the workflow to enable the schedule.

Step-by-step setup (detailed)

1) Clone the repo (optional)

   git clone https://github.com/VisionStack-404/daily-commit.git
   cd daily-commit

2) Review the files
- Open `n8n/Github-actions-2026-08-28.sanitized.json` to inspect the nodes and logic.
- View `assets/workflow-diagram.svg` for a visual layout of the nodes and connections.

3) Import the workflow into n8n
- Open the n8n Editor at your instance (local or n8n.cloud).
- From the top-left menu: Import → choose file → upload `n8n/Github-actions-2026-08-28.sanitized.json`.
- A workflow named "Github actions" (or similar) will appear in the editor.

4) Create and configure n8n credentials
- GitHub credential (recommended: Personal Access Token)
  - Create a PAT with scope: repo (or at least repo:status, repo_deployment, public_repo, repo:invite as needed).
  - In n8n: Settings → Credentials → Create new credential → choose GitHub and paste the PAT.
- OpenAI credential
  - Copy your OpenAI API key (or equivalent provider key).
  - In n8n: Create an OpenAI credential and paste the key.

5) Update workflow node settings (critical)
- Get a file (GitHub node)
  - Set Owner: your GitHub username or organization.
  - Repository: the repository that contains the `Daily` file.
  - File path: `Daily` (or change to `daily.md` or `diary/Daily` if you prefer a path).
  - Credential: select the GitHub credential you created.
- Edit a file (GitHub node)
  - Ensure the target repository and file path match your intent.
  - Commit message: customize to your preference (e.g., "docs: update Daily with automated entry").
  - Credential: select your GitHub credential.
- OpenAI Chat Model / AI Agent node
  - Select the OpenAI credential (or other LLM credential) you created.
  - Review the system prompt included in the node options — this controls how entries are appended and must be preserved for the desired behavior.

6) Sanity check and test run
- Save the workflow in the n8n Editor.
- With the workflow open, use "Execute workflow" (manual run) to test.
- Monitor the execution logs at the bottom panel. Look for:
  - Successful read of the target file.
  - AI Agent returning updated content.
  - Successful write/commit back to the repository.
- Check the GitHub repository to confirm the `Daily` file was updated and a new commit appears.

7) Enable schedule and monitoring
- If the manual test succeeds, activate the workflow in n8n to enable the schedule trigger.
- Check n8n executions or set up alerts (optional) to notify you on failures.

Security and privacy best practices
- This repository contains a sanitized workflow; real credential values were removed. Do not publish PATs, API keys, or webhook secrets in public repositories.
- Use n8n's credential manager — do not insert secrets directly into workflow JSON when possible.
- For production, prefer an OAuth app for GitHub or a machine user with a scoped PAT limited to required permissions.
- Rotate credentials regularly.

Troubleshooting (common issues)
- Permission denied when reading or writing the file: confirm the PAT has repo write permissions and the owner/repo fields are correct.
- LLM errors or timeouts: check the OpenAI (or provider) usage limits and API key validity.
- Workflow import errors: ensure you imported the correct JSON file and your n8n version is compatible with the node types used.

Optional enhancements
- Change the schedule frequency: open the Schedule Trigger node and edit the interval (e.g., daily or hourly).
- Use a dated filename to keep historical logs (e.g., `Daily-2026-08-28.md`) instead of a single `Daily` file.
- Store entries in a dedicated folder and create a simple index.
- Add automatic pull request creation instead of direct push if you want review before merging.

Restoring webhook/webhookId or original credential IDs (if you need them)
- If you used webhooks and need the webhookId value, re-create the webhook inside your private n8n instance rather than pasting webhook IDs into public files.
- When you recreate credentials in n8n, n8n will populate internal IDs automatically; you do not need to edit the JSON to reference them.

Files in this repo to reference
- n8n/Github-actions-2026-08-28.sanitized.json — import to n8n.
- assets/workflow-diagram.svg — visual overview.

Need me to do this for you?
I can perform any of the following on your behalf:
- Create a sample `Daily` file in this repository so new users have a test target.
- Add a CONTRIBUTING.md or further documentation for maintainers.
- Create a branch for changes instead of updating `main` directly.
- Make the repository private.

If you want me to commit one of those changes now, tell me which and I will proceed.