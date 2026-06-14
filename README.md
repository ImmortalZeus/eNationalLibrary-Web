# eNationalLibrary

Central repository for the **eNationalLibrary** web application, a group project for the *Introduction to Software Engineering* course.

This repository is a lightweight container that pulls in the two real codebases as **Git submodules**:

- [`backend/`](https://github.com/ImmortalZeus/eNationalLibrary-Web-Backend) — NestJS REST API (PostgreSQL, JWT auth)
- [`frontend/`](https://github.com/ImmortalZeus/eNationalLibrary-Web-Frontend) — React 19 + Vite SPA

> The actual source code does **not** live in this repository. Clone the submodules (see below) to get a runnable copy.

---

## 🧱 Architecture

```
┌────────────────────┐      HTTP / JSON       ┌────────────────────┐
│  React + Vite SPA  │ ─────────────────────▶ │  NestJS REST API   │
│   (frontend/)      │  Bearer JWT in header  │   (backend/)       │
│   port 5173        │ ◀───────────────────── │   port 3000        │
└────────────────────┘                        └────────┬───────────┘
                                                       │ TypeORM
                                                       ▼
                                              ┌────────────────────┐
                                              │   PostgreSQL DB    │
                                              └────────────────────┘
```

- The frontend never talks to the database directly — everything goes through the backend's REST API.
- Authentication is JWT-based. The token is stored in the browser's `localStorage` and attached to every API call by an Axios interceptor.
- CORS on the backend is preconfigured to accept requests from the frontend's Vite dev server (`http://localhost:5173`).

---

## 📁 Repository Structure

```
eNationalLibrary/
├── backend/      # Git submodule → eNationalLibrary-Web-Backend
├── frontend/     # Git submodule → eNationalLibrary-Web-Frontend
├── .gitmodules
├── CLAUDE.md
└── README.md
```

The submodules are pinned to specific commits. After cloning this repo you must initialize and update them before anything will work.

---

## ✅ Prerequisites

You'll need the following to run the full stack locally:

- **Git** — to fetch the submodules
- **Node.js** ≥ 18 and **npm** ≥ 9
- **PostgreSQL** ≥ 14, running locally on port `5432`

---

## 🚀 Quick Start

### 1. Clone with submodules

```bash
git clone https://github.com/ImmortalZeus/eNationalLibrary.git
cd eNationalLibrary
git submodule update --init --recursive
```

If you already cloned without `--recurse-submodules`, this command fetches everything you need.

### 2. Install dependencies for both components

```bash
# Backend
cd backend
npm install

# Frontend
cd ../frontend
npm install
```

### 3. Configure environment variables

The backend needs a `.env` file. A template is provided at `backend/.env.example`:

```bash
cp backend/.env.example backend/.env
```

Then edit `backend/.env` and fill in your PostgreSQL credentials and a `JWT_SECRET`.

The frontend does not require any environment variables by default. It will connect to `http://localhost:3000/api` out of the box. To override that, create `frontend/.env`:

```env
VITE_API_URL=http://localhost:3000/api
```

### 4. (Optional) Seed sample data

```bash
# Create a default admin account
node backend/scripts/seed-admin.js
# → email: test@test.com, password: test

# Then start the backend and populate sample books/authors/etc.
```

### 5. Run both services

Open two terminals:

```bash
# Terminal 1 — backend
cd backend
npm run start:dev
# → http://localhost:3000/api
```

```bash
# Terminal 2 — frontend
cd frontend
npm run dev
# → http://localhost:5173
```

Open `http://localhost:5173` in your browser. You should see the eNationalLibrary home page.

---

## 🔄 Updating the Submodules

The submodules are pinned to specific commits in their own repositories. To pull the latest changes from each:

```bash
# Update both to the latest commit on their default branch
git submodule update --remote --merge

# Or update a specific one
cd backend
git pull origin main
cd ..
git add backend
git commit -m "chore: update backend submodule to latest commit"
```

> A new commit in this main repository just means a submodule pointer was moved. The submodule repositories themselves hold the actual history.

---

## 📖 Per-Component Documentation

For setup details, API reference, and module listings, see the README in each submodule:

- **Backend** → [backend/README.md](./backend/README.md) or [on GitHub](https://github.com/ImmortalZeus/eNationalLibrary-Web-Backend)
- **Frontend** → [frontend/README.md](./frontend/README.md) or [on GitHub](https://github.com/ImmortalZeus/eNationalLibrary-Web-Frontend)

---

## 👥 Group Members

- Đặng Trung Anh — 20235583
- Hoàng Gia Nam Anh — 20235584
- Phạm Đức Duy — 20235588
- Nguyễn Thái Anh Minh — 20235605
- Trần Tiến Sơn — 20235620

---

✍️ *Group project for the Introduction to Software Engineering course.*
