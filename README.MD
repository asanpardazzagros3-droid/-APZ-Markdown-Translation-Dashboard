APZ Markdown Dashboard

A multilingual documentation management system with built-in translation workflow, diff tracking, and notification support.

---

📖 Overview

APZ Markdown Dashboard is a complete platform for managing, translating, and publishing documentation in multiple languages. It provides:

· A web dashboard to browse and manage documents.
· An API for handling translations, diff generation, and notifications.
· Real-time diff between original and translated versions.
· Integration with Telegram and Discord for approval/update alerts.
· GitHub Actions for automated deployment.

Designed for teams that need to maintain up‑to‑date documentation across languages with minimal friction.

---

✨ Features

· Multi‑language support – English, Persian (Farsi), Kurdish (Kurmanji) – easily extendable.
· Document versioning – Track changes between source and translated files.
· Visual diff viewer – See exactly what changed in each translation.
· Admin panel – Manage documents, translations, approvals, and view diffs.
· Notification system – Get alerted on Telegram/Discord when translations are updated or need review.
· Docker‑ready – Run the entire stack with docker-compose.
· CI/CD – Automatic deployment via GitHub Actions (pages + server).

---

🧱 Project Structure

```
apz-markdown-dashboard/
├── apps/
│   ├── api/                     # Backend API (Node.js + Express)
│   │   ├── index.js             # Entry point
│   │   ├── routes/              # API routes
│   │   │   ├── docs.js          # Document endpoints
│   │   │   ├── translations.js  # Translation management
│   │   │   └── notify.js        # Notification dispatcher
│   │   ├── notify/              # Notification implementations
│   │   │   ├── telegram.js
│   │   │   ├── discord.js
│   │   │   ├── messages.js      # Message templates
│   │   │   └── translationStatus.js
│   │   ├── utils/diff.js        # Diff generation logic
│   │   ├── .env.example         # Environment variables template
│   │   └── Dockerfile
│   └── web/                     # Frontend (Next.js)
│       ├── pages/               # Next.js pages
│       │   ├── index.js         # Home page
│       │   ├── docs/[slug].js   # Document view
│       │   └── admin/           # Admin pages
│       │       ├── translations.js
│       │       ├── docs.js
│       │       ├── approvals.js
│       │       └── diff.js
│       ├── components/          # Reusable UI components
│       │   ├── LanguageSwitcher.js
│       │   └── DiffViewer.js
│       ├── lib/i18n.js          # Internationalisation setup
│       ├── locales/             # Translation files for UI
│       │   ├── fa.json
│       │   ├── en.json
│       │   └── ku.json
│       └── Dockerfile
├── docs/                        # Live documentation (served to users)
│   ├── fa/
│   ├── en/
│   └── ku/
├── docs_pending/                # Pending translation drafts (awaiting review)
│   ├── fa/
│   ├── en/
│   └── ku/
├── .github/
│   └── workflows/               # GitHub Actions CI/CD
│       ├── deploy-pages.yml     # Deploy frontend to GitHub Pages
│       └── deploy-server.yml    # Deploy backend to server
├── ecosystem.config.js          # PM2 configuration
├── docker-compose.yml           # Multi‑container setup
├── README.md
├── CONTRIBUTING.md
├── docs/
│   ├── monitoring.md            # Monitoring guide
│   └── server-test-checklist.md # Server validation checklist
└── .gitignore
```

---

🚀 Getting Started

Prerequisites

· Node.js (v18 or later)
· npm / yarn / pnpm
· Docker (optional, for containerised deployment)
· A GitHub account (for CI/CD)

Installation

1. Clone the repository
   ```bash
   git clone https://github.com/your-org/apz-markdown-dashboard.git
   cd apz-markdown-dashboard
   ```
2. Install dependencies (for both API and Web)
   ```bash
   cd apps/api && npm install
   cd ../web && npm install
   ```
3. Set up environment variables
   Copy .env.example to .env in the api folder and fill in your values:
   ```bash
   cp apps/api/.env.example apps/api/.env
   ```
   Required variables:
   Variable Description
   DOCS_PATH Absolute path to the docs/ folder
   PENDING_PATH Absolute path to the docs_pending/ folder
   TELEGRAM_BOT_TOKEN Optional – Telegram bot token
   TELEGRAM_CHAT_ID Optional – Telegram chat ID
   DISCORD_WEBHOOK Optional – Discord webhook URL
   PORT API port (default: 3001)
4. Run the development servers
   · API (from apps/api):
     ```bash
     npm run dev
     ```
   · Web (from apps/web):
     ```bash
     npm run dev
     ```
   The web app will be available at http://localhost:3000 and the API at http://localhost:3001.

---

🐳 Docker Deployment

For production or testing, use Docker Compose:

```bash
docker-compose up -d
```

This will build and run both the API and web containers. The web app will be exposed on port 80 (or configured in docker-compose.yml).

---

🔧 Configuration

Environment Variables (API)

Variable Default Description
DOCS_PATH – Path to live documentation (e.g., ./docs)
PENDING_PATH – Path to pending translations (./docs_pending)
TELEGRAM_BOT_TOKEN "" Telegram bot token (leave empty to disable)
TELEGRAM_CHAT_ID "" Target chat ID for Telegram notifications
DISCORD_WEBHOOK "" Discord webhook URL (leave empty to disable)
PORT 3001 API server port

Web App Locales

Add new languages by creating a JSON file in apps/web/locales/ and updating lib/i18n.js. The UI will automatically pick up the new language.

---

📡 API Endpoints

All endpoints are prefixed with /api/v1 (check routes/ for exact versions).

Documents

· GET /docs – List all documents (with status per language).
· GET /docs/:slug – Retrieve a specific document in the requested language (via Accept-Language header or query param).

Translations

· GET /translations/:slug – Get translation status and diff for a given document.
· POST /translations/:slug – Submit a new translation draft (stored in docs_pending/).
· POST /translations/:slug/approve – Approve a pending translation (moves it to docs/).
· POST /translations/:slug/reject – Reject a pending translation (deletes it).

Notifications

· POST /notify/translate – Manually trigger a notification for a translation update.
· POST /notify/approve – Notify about an approval.

For full details, refer to the API documentation (generated via Swagger – coming soon).

---

🔔 Notifications

The system can send alerts via Telegram and/or Discord when:

· A new translation is submitted.
· A translation is approved.
· A document is updated.

Configure the appropriate environment variables to enable these channels. If both are set, notifications are sent to both.

---

🧪 Testing & Monitoring

· Server test checklist – See docs/server-test-checklist.md for a step‑by‑step validation guide.
· Monitoring – Refer to docs/monitoring.md for recommended health checks and logging practices.

---

🚢 Deployment (GitHub Actions)

The repository includes two workflows:

1. deploy-pages.yml – Builds the web app and deploys it to GitHub Pages.
2. deploy-server.yml – Deploys the API to a server (requires SSH and PM2 setup).

Adjust the workflows to match your hosting environment.

---

🤝 Contributing

We welcome contributions! Please read our CONTRIBUTING.md for guidelines on:

· Reporting issues
· Submitting pull requests
· Adding new languages
· Code style and testing

---

📄 License

This project is licensed under the MIT License – see the LICENSE file for details.

---

🙏 Acknowledgements

Built with:

· Next.js
· Express
· Node.js
· Docker
· GitHub Actions

---

Ready for global release.
For questions or support, please open an issue or contact the maintainers.
