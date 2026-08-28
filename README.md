# n8n — Daily Commit Workflow

This repository contains an n8n workflow that updates a "Daily" file in a GitHub repository on a schedule. It was sanitized before publishing to remove credential IDs and webhook identifiers. Use this as a small demo / mini-project showing how to automate periodic updates to a text file in a GitHub repo using n8n and an LLM.

Files included
- n8n/Github-actions-2026-08-28.sanitized.json — the exported workflow JSON with credential placeholders.
- assets/workflow-diagram.svg — a simple SVG diagram visualizing the workflow nodes and connections.

What this workflow does (high level)
1. Schedule Trigger (every 3 days) — starts the workflow on a schedule.
2. Get a file — reads a file named `Daily` from a GitHub repository.
3. Extract from File — extracts the text content.
4. AI Agent — calls a language model with system instructions to append a new daily entry (without modifying earlier entries).
5. Convert to File — converts the model output to a file payload.
6. Edit a file — writes the updated `Daily` file back to the GitHub repo with a commit message.

Security notes (important)
- This public copy removes sensitive webhook IDs and credential identifiers. Do NOT paste real API keys, personal access tokens, or webhook secrets into files you publish.
- In n8n, credentials (GitHub, OpenAI) should be created using the n8n credentials manager and NOT stored in the workflow JSON in plaintext.
- Replace placeholders in the imported workflow with your own credentials inside your private n8n instance.

Quickstart — how to run locally or in n8n.cloud
1. Clone this repo:

   git clone https://github.com/VisionStack-404/daily-commit.git
   cd daily-commit

2. Open n8n (local or n8n.cloud) and import the workflow JSON:
   - In n8n Editor, click the folder icon (top-left) → Import → choose `n8n/Github-actions-2026-08-28.sanitized.json`.

3. Create credentials in n8n:
   - GitHub: create a GitHub API credential using a Personal Access Token (repo scope) or connect via OAuth.
   - OpenAI: create an OpenAI credential with your API key.

4. Update the workflow nodes (inside Editor):
   - Open the `Get a file` and `Edit a file` nodes and set the repository/owner and file path if you want to target a different repo or path.
   - In any node shown with a credential placeholder, select the credentials you created in step 3.

5. Sanity check and test:
   - Run the workflow manually using the "Execute workflow" button to test behavior before enabling the schedule.
   - Confirm the `Daily` file was updated in the target repository.

6. Enable schedule (optional):
   - Once tested, activate the workflow and it will run on the configured schedule.

How to restore webhook/webhookId fields (if you need them)
- If you previously used webhooks in n8n for real-time triggers, do not paste webhook IDs into a public commit. Re-create the credential or webhook in your private n8n instance and let n8n populate the webhookId automatically.

Diagram and explanation
- See `assets/workflow-diagram.svg` for a visual overview of the nodes and connections.

Customization ideas
- Change schedule frequency (Schedule Trigger node) to daily or hourly.
- Use a different prompt or logic in the AI Agent node to modify the style of entries.
- Write to a dated filename instead of a single `Daily` file.

License
- This repository is provided as-is for demonstration. Add a license file if you want to reuse this in other projects.

If you want, I can now:
- Commit the exact (unsanitized) workflow JSON instead (not recommended public).
- Create a branch instead of committing to `main`.
- Add a short example `Daily` file and a sample execution log.

Tell me which of these you want next, or I can continue with further changes automatically.
