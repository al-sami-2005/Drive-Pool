# DriveCloud

Unified dashboard that aggregates N Google Drive accounts into a single storage pool. Files are automatically routed to the account with the most available space on every upload.

## Structure

```
DriveCloud/
├── backend/          FastAPI + SQLite
├── frontend/         Next.js (App Router) + Tailwind CSS
└── config/           Google OAuth credentials (not committed)
```

## Setup

### 1 — Google OAuth credentials

For each Google account, create an OAuth 2.0 client in [Google Cloud Console](https://console.cloud.google.com/) (type: **Web application**) and add `http://localhost:8000/api/auth/callback` as an authorized redirect URI.

Download the JSON and save it as `config/credentials_1.json`, `config/credentials_2.json`, etc. The number of accounts is detected automatically.

### 2 — Backend environment

```bash
cd backend
cp .env.example .env
uv run python scripts/generate_secrets.py
```

Paste the output into `backend/.env`.

### 3 — Run the backend

```bash
cd backend
uv run uvicorn main:app --reload --app-dir ..
```

Or without uv:

```bash
pip install -r backend/requirements.txt
uvicorn backend.main:app --reload
```

API docs available at `http://localhost:8000/docs`.

### 4 — Run the frontend

```bash
cd frontend
npm install
npm run dev
```

Dashboard at `http://localhost:3000`.

### 5 — Connect accounts

Log in with your PIN, then click **Connect** on each account card to complete the Google OAuth flow.

## Features

- Dynamic account count — add or remove `credentials_*.json` files and restart the backend
- Least Used Space upload routing — files go to the account with the most free space
- Thumbnails proxied from Drive (no full-file fetch)
- Inline file rename, download, and delete
- PIN-protected with bcrypt + httponly JWT cookie
