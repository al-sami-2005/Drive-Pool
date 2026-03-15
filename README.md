# DrivePool · Built by Al Sami

Self-hosted unified dashboard that aggregates multiple Google Drive accounts into a single storage pool. Every upload automatically routes to the account with the most available space — no manual management, no paid tier.

> **Runs entirely on your local machine. Your files stay in your own Google Drive accounts.**

---

## Why DrivePool?

Google gives every account **15 GB free**. DrivePool lets you combine as many accounts as you want into one unified interface — effectively giving you N × 15 GB of free cloud storage. Add more accounts at any time without changing any configuration.

---

## Features

- **Unified storage pool** — one dashboard for all your Drive accounts
- **Smart upload routing** — Least-Used-Space strategy picks the best account on every upload
- **Folder navigation** — full hierarchy, breadcrumbs, grid & list views, search, filters
- **Drag-to-folder** — drag a file and drop it into any folder from a slide-in panel
- **Shared with me** — browse and download files others have shared with your accounts
- **Trash management** — delete goes to Drive trash; restore or permanently delete anytime
- **Analytics** — storage by account, file type charts, weekly upload activity
- **Profile** — display name, bio, avatar stored in your own Drive
- **Secure** — bcrypt-hashed PIN, httponly JWT cookie, OAuth tokens encrypted at rest (Fernet)
- **No .env needed** — secrets are stored directly in the local SQLite database
- **Dark / light theme**
- **100% open source, $0 cost**

---

## Quick Start

**Prerequisites:** Python 3.10+, Node.js 18+, at least one Google account

### 1. Setup

```bash
cd Drive-Pool
```

### 2. Google OAuth credentials

You only need **one Google Cloud project** and one credentials file — no matter how many Drive accounts you add.

1. Go to [Google Cloud Console](https://console.cloud.google.com) → create a new project
2. Enable **Google Drive API**
3. Create an OAuth consent screen → choose **External**, fill in any app name, and add **all Google accounts you want to connect** as test users
4. Create credentials → **OAuth client ID → Web application** → add `http://localhost:8000/api/auth/callback` as an authorized redirect URI → download JSON
5. Save as `config/credentials.json`

### 3. Install & set up

```bash
pip install -r backend/requirements.txt
python backend/scripts/generate_secrets.py
```

Enter a PIN when prompted — your PIN hash, JWT secret, and encryption key are written directly to `backend/drivepool.db`. No `.env` file needed.

### 4. Start the servers

```bash
# Terminal 1 — backend
uvicorn backend.main:app --reload

# Terminal 2 — frontend
cd frontend && npm install && npm run dev
```

### 5. Connect your accounts

Open [http://localhost:3000](http://localhost:3000), log in with your PIN, navigate to **Settings**, and click **Connect another account** to authorize each Google account via OAuth.

That's it — start uploading at [http://localhost:3000/dashboard](http://localhost:3000/dashboard).

---

## Adding more storage

Go to **Settings** and click **Connect another account** — no file changes, no restart needed. Make sure the new Google account is added as a test user on the OAuth consent screen first. Each free Google account adds 15 GB to your pool.

---

## Tech stack

| Layer | Tech |
|-------|------|
| Backend | Python · FastAPI · SQLite · Google Drive API v3 |
| Frontend | Next.js (App Router) · Tailwind CSS |

---

## Full setup guide

See [http://localhost:3000/docs](http://localhost:3000/docs) once the app is running, or read `frontend/app/docs/page.tsx` directly.

---

## License

MIT
