# Workforce Management System (WMS) — College Project Setup Guide

Welcome to the Workforce Management System project repository. This guide covers how to set up and run the backend and frontend applications locally.

---

## 📁 Repository Structure

```text
ssv/
├── admin-panel/         # Admin Web Dashboard (React + Vite + Material UI)
├── admin-panel-2/       # Admin Web Dashboard 2 (React + Vite + Material UI)
├── Backend/             # Main REST API Backend (NestJS + Prisma ORM + PostgreSQL)
│   └── flutter_app/     # Worker Mobile Application (Flutter)
├── backend-2/           # Backend Variant 2 (NestJS + Prisma ORM + PostgreSQL)
│   └── flutter_app/     # Worker Mobile Application Variant 2 (Flutter)
├── supervisor-panel/    # Supervisor Web Dashboard (React + Vite + Material UI)
└── user/                # Worker / Employee Web Dashboard (React + Vite + Material UI)
```

---

## ⚙️ Prerequisites

1. **Node.js**: `v20.x` or higher (Node LTS recommended)
2. **PostgreSQL**: PostgreSQL 15+ running locally (or via Docker)
3. **Flutter SDK**: (Optional, only if running the mobile application)

---

## 🚀 1. Backend Setup (`Backend/` or `backend-2/`)

### Step 1: Navigate to the backend directory
```bash
cd Backend
# or cd backend-2
```

### Step 2: Install dependencies
```bash
npm install
```

### Step 3: Configure environment variables
Create a `.env` file in `Backend/` (copied from `.env.example`):
```env
APP_NAME=wms-backend
APP_PORT=3001
NODE_ENV=development
DATABASE_URL="postgresql://postgres:postgres@localhost:5432/wms_db?schema=public"
JWT_SECRET=wms_jwt_secret_dev_key_12345
JWT_EXPIRES_IN=1d
```
*(Make sure to adjust the `DATABASE_URL` with your local PostgreSQL username, password, and database name).*

### Step 4: Run database migrations & seed sample data
```bash
# Generate Prisma Client & apply database migrations
npx prisma migrate dev --name init

# Seed initial roles, departments, activities, and demo users
node seed-all-data.js
```

### Step 5: Start the backend server
```bash
npm run start:dev
```
The backend API will be available at: **`http://localhost:3001/api/v1`**

---

## 💻 2. Frontend Applications Setup

Each frontend panel can be run independently in separate terminal windows:

### A. Admin Panel (`admin-panel/` or `admin-panel-2/`)
```bash
cd admin-panel
npm install
npm run dev
```
- Runs on: **`http://localhost:5173`**
- Proxies `/api` requests to `http://localhost:3001`

### B. Supervisor Panel (`supervisor-panel/`)
```bash
cd supervisor-panel
npm install
npm run dev
```
- Runs on: **`http://localhost:5174`**
- Proxies `/api` requests to `http://localhost:3001`

### C. Worker / User Panel (`user/`)
```bash
cd user
npm install
npm run dev
```
- Runs on: **`http://localhost:5175`**
- Proxies `/api` requests to `http://localhost:3001`

---

## 📱 3. Mobile Application (`Backend/flutter_app/`)

```bash
cd Backend/flutter_app
flutter pub get
flutter run
```
- Configured in `lib/app/config.dart` to communicate with `http://localhost:3001/api/v1`.

---

## 🔑 Demo Login Credentials (from local seed)

| Role | Employee ID | Default Password |
| :--- | :--- | :--- |
| **System Admin** | `ADMIN001` | `Admin@123` |
| **Admin User** | `EMP-1042` | `Admin@123` |
| **Supervisor** | See seeded supervisor list | `password123` or `Admin@123` |
| **Worker** | See seeded worker list | `password123` or `Admin@123` |
