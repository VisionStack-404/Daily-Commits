# 🚀 Daily Commit Automation

> **An AI-powered n8n workflow that maintains a daily learning log and commits meaningful progress to GitHub automatically.**

<p align="center">

![GitHub](https://img.shields.io/badge/GitHub-Repository-181717?style=for-the-badge&logo=github)
![n8n](https://img.shields.io/badge/n8n-Automation-EA4B71?style=for-the-badge&logo=n8n)
![OpenAI](https://img.shields.io/badge/OpenAI-AI_Agent-412991?style=for-the-badge&logo=openai)
![GitHub API](https://img.shields.io/badge/GitHub_API-Integrated-181717?style=for-the-badge&logo=github)
![Status](https://img.shields.io/badge/Status-Active-success?style=for-the-badge)

</p>

---

## 📌 Overview

**Daily Commit Automation** is an AI-assisted GitHub workflow built with **n8n**.

Its purpose is simple:

> **Learn → Document → Automate → Commit → Repeat**

Instead of manually maintaining a daily learning log, the workflow reads an existing `Daily` file, sends its contents to an AI Agent, generates a dated progress entry, converts the result into a file, and commits it to GitHub automatically.

The workflow is designed to keep the process consistent while keeping credentials and secrets outside the repository.

---

## 🔄 How It Works

### Workflow Steps

Each day, the automation follows this sequence:

| Step | Component | Input | Output |
|------|-----------|-------|--------|
| 1️⃣ | ⏰ **Schedule Trigger** | Daily time | Workflow starts |
| 2️⃣ | 📂 **Get a File** | Repository + path | Daily file content |
| 3️⃣ | 🔎 **Extract From File** | Raw file | Clean text |
| 4️⃣ | 🤖 **AI Agent** | File content + rules | Generated entry |
| 5️⃣ | 🧾 **Convert To File** | AI output | File payload |
| 6️⃣ | ✏️ **Edit a File** | Updated content | GitHub commit |

### Visual Workflow

```text
                     🚀 DAILY COMMIT AUTOMATION
                              │
                              ▼
                ┌──────────────────────────┐
                │   ⏰ Schedule Trigger    │
                │     (Runs daily)        │
                └────────────┬─────────────┘
                             │
                             ▼
                ┌──────────────────────────┐
                │    📂 Get a File        │
                │   Read Daily content    │
                └────────────┬─────────────┘
                             │
                             ▼
                ┌──────────────────────────┐
                │  🔎 Extract From File   │
                │    File → Clean Text    │
                └────────────┬─────────────┘
                             │
                             ▼
                ┌──────────────────────────┐
                │    🤖 AI Agent          │
                │   Analyze + Generate    │
                │   Today's entry         │
                └────────────┬─────────────┘
                             │
                             ▼
                ┌──────────────────────────┐
                │  🧾 Convert To File     │
                │    Text → File Payload  │
                └────────────┬─────────────┘
                             │
                             ▼
                ┌──────────────────────────┐
                │    ✏️ Edit a File       │
                │   Update + Create       │
                │   GitHub commit         │
                └────────────┬─────────────┘
                             │
                             ▼
                       ✅ Commit Complete
```

---

## 🧠 AI Agent

The AI Agent is responsible for maintaining the daily learning log.

### Core Rules

✅ Preserve previous entries  
✅ Append today's entry  
✅ Keep the format consistent  
✅ Keep the information factual  
✅ Avoid duplicate entries  
✅ Do not invent achievements  
✅ Return only the updated file content  

### Recommended Prompt

```
Read the existing daily learning file content provided by the previous node.

Your task is to maintain this file as a clean daily learning and progress log.

1. Preserve all existing entries exactly as they are.
2. Do NOT delete, rewrite, or modify previous entries.
3. Add a new entry for today's date at the end of the file.
4. Keep the formatting professional, simple, and consistent with the existing file.
5. Do not invent projects, achievements, technologies, tasks, or facts.
6. Today's progress: Continue documenting my daily software engineering, computer science, DSA, machine learning, and GitHub learning progress.
7. If an entry for today's date already exists, do not create a duplicate.
8. Return ONLY the complete updated daily learning file content.
9. Do NOT include explanations.
10. Do NOT use a markdown code block.

Existing daily learning file content:
{{ $json.text }}

Today's date:
{{ $now.format('yyyy-MM-dd') }}
```

---

# 🧭 Workflow Architecture

```text
                    ┌──────────────────┐
                    │     USER         │
                    │  Daily Learning  │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │       n8n        │
                    │   Orchestrator   │
                    └────────┬─────────┘
                             │
                ┌────────────┴────────────┐
                │                         │
                ▼                         ▼
       ┌────────────────┐       ┌────────────────┐
       │    GitHub      │       │     OpenAI     │
       │  Read / Write  │       │   AI Agent     │
       └────────┬───────┘       └────────┬───────┘
                │                        │
                └────────────┬───────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │   Daily File     │
                    │      📝          │
                    └────────┬─────────┘
                             │
                             ▼
                       ✅ Git Commit
```

---

## 🛠️ Tech Stack

```
┌────────────────────────────────────────────┐
│                TECHNOLOGY                  │
├────────────────────────────────────────────┤
│                                            │
│  ⚙️ n8n             → Workflow Automation │
│  🐙 GitHub          → Repository + Commits │
│  🔌 GitHub API      → Read / Write Files   │
│  🤖 OpenAI          → AI Agent             │
│  📄 Markdown        → Learning Log        │
│  🧩 JSON            → Workflow Export      │
│                                            │
└────────────────────────────────────────────┘
```

---

## 📁 Repository Structure

```
daily-commit/
│
├── 📄 README.md
│
├── 🤖 n8n/
│   └── Github-actions-2026-08-28.sanitized.json
│
├── 🎨 assets/
│   ├── workflow-diagram.svg
│   └── original-workflow-diagram.svg
│
├── 📝 examples/
│   └── Daily.md
│
├── ⚙️ .github/
│   └── workflows/
│       └── validate.yml
│
├── 🔒 .gitignore
├── 🤝 CONTRIBUTING.md
└── 📜 LICENSE
```

---

## ⚙️ Setup

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/VisionStack-404/daily-commit.git
cd daily-commit
```

### 2️⃣ Open n8n

Open your n8n instance and create a new workflow.

Import the sanitized workflow:

```
n8n/
└── Github-actions-2026-08-28.sanitized.json
```

### 3️⃣ Configure GitHub Credentials

Create a GitHub credential inside n8n.

**Recommended approach:**

```
GitHub
   ↓
Fine-Grained Personal Access Token
   ↓
Repository access
   ↓
Contents: Read and Write
```

Use the credential in:

- 📂 **Get a file** → GitHub credential
- ✏️ **Edit a file** → GitHub credential

### 4️⃣ Configure OpenAI

Add your OpenAI credential to the Chat Model used by the AI Agent.

**Example:**

```
AI Agent
   │
   └── OpenAI Chat Model
           │
           └── gpt-4-mini
```

⚠️ **Never commit the API key to GitHub.**

---

## 🔐 Security

Credentials should never be stored inside this repository.

### ✅ Safe to Commit
- Sanitized n8n workflow
- Documentation
- Diagrams
- Example files
- CI configuration

### ❌ Never Commit
- GitHub Personal Access Tokens
- OpenAI API keys
- n8n credential secrets
- `.env` files
- Webhook secrets
- Passwords

### Example `.gitignore`

```gitignore
# Environment files
.env
.env.*
*.env

# Secrets
secrets/
credentials/
*.key
*.pem

# Local files
.DS_Store
Thumbs.db

# Logs
*.log

# n8n local data
.n8n/
```

---

## 🧪 Testing

Before enabling the daily schedule, follow these steps:

1. Import workflow
2. Configure credentials
3. Select repository
4. Select Daily file
5. Execute workflow manually
6. Check every node
7. Verify GitHub commit
8. Enable schedule

### Expected Result

```
Schedule Trigger      ✅
        ↓
Get a File            ✅
        ↓
Extract From File     ✅
        ↓
AI Agent              ✅
        ↓
Convert To File       ✅
        ↓
Edit a File           ✅
        ↓
GitHub Commit         ✅
```

---

## 📅 Daily File

The automation maintains a file named:

- `Daily` or `Daily.md`

### Example Format

```markdown
# 📚 Daily Learning Log

## 2026-08-28

- Continued learning software engineering concepts.
- Practiced DSA and machine learning concepts.
- Worked with GitHub and n8n automation.
- Improved workflow automation and documentation.

## 2026-08-29

- Enhanced workflow architecture documentation.
- Refined AI Agent prompting strategy.
- Improved GitHub commit automation workflow.
```

---

## ⏰ Scheduling

The workflow can be configured to run daily at a specific time:

**Example Configuration:**

```
Frequency: Daily
Time: 21:00
Timezone: Asia/Calcutta
```

---

## 🚧 Important Design Principle

This project should **not** be used to create meaningless activity simply to generate GitHub contribution squares.

The intended workflow is:

```
REAL LEARNING
     ↓
REAL PROGRESS
     ↓
AI DOCUMENTATION
     ↓
REAL GITHUB COMMIT
```

The AI should document genuine progress rather than manufacture achievements.

---

## 🔮 Future Improvements

### 🧠 Smarter GitHub Activity Analysis

Instead of using a generic daily statement, future versions can inspect:

- GitHub commits
- Pull requests
- Issues
- Changed files
- Repository activity

and generate the daily log from actual work.

### 📆 Per-Day Files

Instead of one growing file, the workflow can create:

```
daily/
├── 2026-08-28.md
├── 2026-08-29.md
├── 2026-08-30.md
└── ...
```

This keeps individual files small and easier to maintain.

### 🔀 Pull Request Workflow

A more advanced architecture:

```
AI Agent
   ↓
Create Branch
   ↓
Update Daily
   ↓
Create Pull Request
   ↓
Review
   ↓
Merge
```

This provides a cleaner software-development workflow.

### 🔔 Notifications

Future versions can send notifications through:

- 📧 Email
- 💬 Slack
- 📱 Discord

when:

- ✅ Daily update succeeds
- ❌ Workflow fails
- ⚠️ GitHub credential expires

---

## 🎯 Project Goals

🎓 Build consistent learning habits  
🤖 Learn practical AI automation  
⚙️ Understand n8n workflows  
🐙 Automate GitHub operations  
🧠 Practice AI-assisted development  
📚 Maintain a transparent learning history  
🔐 Follow secure credential practices  

---

## 🧩 Workflow Components

| Component | Purpose |
|-----------|---------|
| ⏰ Schedule Trigger | Starts the workflow on schedule |
| 📂 Get a File | Retrieves Daily file content |
| 🔎 Extract From File | Extracts text from file |
| 🤖 AI Agent | Generates learning entry |
| 🧾 Convert To File | Creates file payload |
| ✏️ Edit a File | Updates GitHub with commit |
| 🐙 GitHub | Stores history |
| 🧠 OpenAI | Generates content |

---

## 📚 Learning Outcomes

By building this project, you learn:

✅ Workflow automation  
✅ API authentication  
✅ GitHub API integration  
✅ AI Agent workflows  
✅ File processing  
✅ Scheduled automation  
✅ Git concepts  
✅ Secure credential handling  
✅ Continuous documentation  

---

## 🤝 Contributing

Contributions are welcome!

```bash
git checkout -b feature/your-feature
```

Make your changes, test the workflow, and open a Pull Request.

**Please keep contributions:**

✅ Documented  
✅ Tested  
✅ Secure  
✅ Focused  
✅ Reproducible  

---

## 📜 License

This project is licensed under the MIT License.

See [LICENSE](LICENSE) for details.

---

## 👤 Built By

**Varun — VisionStack-404**

*Building consistently. Automating intelligently. Learning every day.*

🐙 [GitHub: VisionStack-404](https://github.com/VisionStack-404)

---

<p align="center">

🚀 Learn → 🤖 Automate → 📝 Document → 🐙 Commit → 🔁 Repeat

</p>

<p align="center">

Made with ❤️ using n8n + GitHub + OpenAI

</p>
