# 🚀 Daily Commit — n8n Automated Daily-Updater (Professional Guide)

A polished, professional guide with an emoji-based visual flow and clear, step-by-step instructions to import, configure, test, and operate the n8n workflow that appends a dated entry to a `Daily` file and commits it back to GitHub.

📁 Repository structure
- n8n/
  - Github-actions-2026-08-28.sanitized.json — exported workflow JSON (credential placeholders). Import into n8n Editor.
- assets/
  - workflow-diagram.svg — simplified visual diagram.
  - original-workflow-diagram.svg — detailed node diagram (optional).
- examples/
  - Daily.md — sample `Daily` file for quick testing.
- .github/
  - workflows/ — CI workflow files (validate JSON / markdown).
- README.md — this file.

---

🔍 Overview
This workflow automates periodic updates to a single file named `Daily` in a GitHub repository. It:
1. Reads the `Daily` file from a repo.
2. Sends the content to an AI Agent (LLM) which appends a new dated entry while preserving previous entries.
3. Converts the result into a file payload.
4. Commits the updated file back to GitHub on a configured schedule.

---

🧭 Emoji visual flow (at-a-glance)

Use this compact emoji diagram for quick orientation. The detailed SVG diagram lives in assets/original-workflow-diagram.svg.


  ⏰ Schedule Trigger  ➜  📂 Get a file  ➜  🔎 Extract from file  ➜  🤖 AI Agent  ➜  🧾 Convert to file  ➜  ✏️ Edit a file


Legend (emoji → node)
- ⏰ Schedule Trigger — scheduleTrigger node (intervals)
- 📂 Get a file — GitHub node (read file)
- 🔎 Extract from file — extractFromFile node (text extraction)
- 🤖 AI Agent — LangChain / Chat model node (system prompt + model)
- 🧾 Convert to file — convertToFile node (prepare binary payload)
- ✏️ Edit a file — GitHub node (commit updated file)

---

⚙️ Tech stack
- n8n — visual automation/orchestration platform (workflow editor, credentials manager).
- GitHub API — used by n8n GitHub nodes to read/write repository contents.
- OpenAI (or other LLM provider) — language model for the AI Agent node.
- JSON/YAML — workflow export and CI config formats.
- Git & GitHub — repository hosting, branches, PRs, and access control.

---

🔧 Quick start — import & test (3 steps)
1) Clone repository (optional)

   git clone https://github.com/VisionStack-404/daily-commit.git
   cd daily-commit

2) Import workflow into n8n Editor
- Open n8n → top-left menu → Import → choose `n8n/Github-actions-2026-08-28.sanitized.json`.

3) Create credentials in n8n and attach them to nodes
- GitHub credential (PAT or OAuth) — used by Get a file / Edit a file nodes.
- OpenAI credential (API key) — used by AI Agent / Chat Model node.

Run a manual execution (Execute workflow) before enabling schedule and verify the commit appears in your target repository.

---

🔐 GitHub token generation & n8n credential setup (clean, step-by-step)
1) Optional: create a machine user (recommended)
- Create a separate GitHub account (e.g., automation-n8n-bot) and grant it access to the target repo.

2) Generate a Personal Access Token (PAT)
- GitHub → Settings → Developer settings → Personal access tokens → Generate new token (classic) or create a fine‑grained token.
- Scopes (minimum): repository content read/write (classic: repo). Optionally `workflow` if needed.
- Set expiration and copy the token (you cannot view it again).

3) Add token to n8n
- n8n → Credentials → New → GitHub → Personal Access Token → paste token → save.

4) Add OpenAI key to n8n
- n8n → Credentials → New → OpenAI → paste key → save.

5) Attach credentials in workflow
- Open the imported workflow in n8n Editor and select the saved credentials in the Get a file / Edit a file / AI Agent nodes.

---

🧪 Testing checklist (manual run)
- [ ] Import workflow and attach credentials.
- [ ] In n8n, run Execute workflow manually.
- [ ] Confirm logs show: successful read → AI output → convert → commit.
- [ ] Verify commit appears in target GitHub repo and Daily file updated.

---

🤖 AI Agent — prompt & configuration (recommended)
System prompt (example — place into AI Agent node):

"You are a careful file editor.\n1) Preserve all existing entries exactly.\n2) Append a single new entry for today's date (YYYY-MM-DD) at the end.\n3) Keep entries concise and factual, focused on daily learning/progress.\n4) Do not invent projects, achievements, or personal details.\n5) If today's entry already exists, do nothing.\n6) Return only the complete updated file content with no explanations, no code fences, and no metadata."

Model & settings
- Model: gpt-5-mini (or whichever is available on your account)
- Temperature: 0.0–0.3 (deterministic)
- Max tokens: ensure the model can output the entire file (or switch to dated per-day files)

---

🔒 Security best practices
- Never publish PATs, API keys, or webhook secrets. Use n8n Credentials.
- Prefer fine‑grained tokens or OAuth where possible.
- Use a machine user token with least privileges and set token expiration.
- Rotate tokens regularly and monitor n8n execution logs.

---

📦 Optional enhancements
- Use per-day files (Daily-YYYY-MM-DD.md) to avoid large file outputs.
- Create branch + PR flow rather than committing directly (for reviews).
- Add notifications for failures (Slack, email) and automated archival of old entries.

---

📎 Files referenced
- n8n/Github-actions-2026-08-28.sanitized.json — import to n8n.
- assets/original-workflow-diagram.svg — detailed visual diagram (recommended for docs).
- examples/Daily.md — sample entry (use to test Get a file / Edit a file nodes).

---

If you want, I will now:
- Add examples/Daily.md, CONTRIBUTING.md, LICENSE (MIT), .gitignore, and the CI workflow into a branch (add-samples-and-ci) and open a Pull Request.
- Or paste the contents of each file here for you to add manually.

Tell me which you prefer and I’ll proceed.