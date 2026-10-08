# FIRST FEST 2026 - Production Event Registration System

[![Event: FIRST FEST 2026](https://img.shields.io/badge/Event-FIRST%20FEST%202026-6366f1.svg)](https://first-fest-2026.vercel.app)
[![Stack: React + Node.js + Supabase](https://img.shields.io/badge/Stack-React%20%7C%20Node%20%7C%20Supabase-8b5cf6.svg)](https://supabase.com)
[![Status: Production Ready](https://img.shields.io/badge/Status-Production%20Ready-10b981.svg)]()

> A real, production-ready, mobile-first event registration platform for **FIRST FEST 2026** built with React, Vite, Node.js, Express, and Supabase Cloud PostgreSQL.

---

## 🌟 Architecture & Features

- **Event Branding**: Strict, elegant branding for **FIRST FEST 2026** featuring modern obsidian/glassmorphic aesthetics, vibrant gradient typography, and college-level festival styling.
- **Mobile-First UX**: Zero horizontal scroll, touch-friendly inputs (48px+), searchable dropdowns, and instant inline feedback.
- **International Calling Codes**: Searchable country select with automatic dialing code population (`+91` for India, `+1` for USA, `+44` for UK, `+61` for Australia, `+1` for Canada, etc.).
- **Dynamic India Geolocation**: Comprehensive dataset covering all 28 Indian States & 8 Union Territories; districts dynamically load based on selected state (e.g. Tamil Nadu displays all 38 districts including Chennai, Coimbatore, Madurai, etc.).
- **Smart Date of Birth**: 3 selectable controls (Date, Month, Year 2002–2026) with automatic leap-year handling (Feb 29/28) and impossible/future date rejection.
- **Participation Modes**:
  - **Individual**: Single participant entry.
  - **Team**: 4 to 5 members only. Validates strictly 4 or 5 members (1–3 invalid, 6 not allowed).
- **Approved Events**:
  - *Technical*: Hackathon, Codex, Neutrics AI, Web Development, Debugging, Paper Presentation.
  - *Non-Technical*: Quiz, Photography, Treasure Hunt, Gaming, Other Non-Technical Event.
- **Cloud Database (Supabase PostgreSQL)**:
  - Atomic, concurrency-safe unique Registration ID sequence (`FF2026-0001`, `FF2026-0002`, ...).
  - Normalised tables: `registrations` and `team_members` with Row Level Security (RLS) policies.
- **Admin Dashboard (`/admin`)**:
  - Protected with Supabase Auth (Email & Password).
  - Real-time statistics: Total, Individual, Teams, Today's new registrations.
  - Live search across Name, Registration ID, Email ID, and Phone Number.
  - Multi-criteria filtering by Country, State, District, Department, Participation Type, and Event.
  - RFC 4180 CSV export with team member names included.
- **Success & Confirmation**:
  - Printable official entry ticket pass with QR Code verification and confetti celebration.

---

## 📁 Project Structure

```text
first-fest-2026/
│
├── frontend/                     # React + Vite Client
│   ├── src/
│   │   ├── components/           # RegistrationForm, SuccessView, SearchableSelect,
│   │   │                         # DateOfBirthSelector, AdminDashboard, AdminLogin
│   │   ├── pages/                # HomePage, AdminPage
│   │   ├── data/                 # countries, indianStatesAndDistricts, departments, events
│   │   ├── styles/               # index.css (Production dark modern styling)
│   │   ├── utils/                # supabaseClient.js (Frontend anon auth)
│   │   ├── App.jsx               # Client routing (/ and /admin)
│   │   └── main.jsx
│   ├── index.html                # WhatsApp/OpenGraph preview metadata
│   ├── package.json
│   └── vite.config.js            # Vite API reverse proxy configuration
│
├── backend/                      # Node.js + Express Production API
│   ├── controllers/              # registrationController.js, adminController.js
│   ├── routes/                   # register.js, admin.js
│   ├── middleware/               # authMiddleware.js, rateLimiter.js
│   ├── utils/                    # idGenerator.js, supabaseClient.js, validationRules.js
│   ├── data/                     # Data validation rules & state/district maps
│   ├── server.js                 # Express server with Helmet, CORS, Rate Limiters
│   └── package.json
│
├── supabase/
│   └── schema.sql                # Complete PostgreSQL Cloud schema, tables & RLS
│
├── test/
│   └── test_all_rules.js         # Automated test suite verifying 33+ validation rules
│
├── api/
│   └── index.js                  # Vercel Serverless Function entry point
│
├── vercel.json                   # Vercel cloud deployment routing
├── .env.example                  # Environment variable reference template
├── .gitignore                    # Production gitignore
├── package.json                  # Root orchestration scripts
└── README.md
```

---

## 🚀 Step-by-Step Production Cloud Deployment

### Step 1: Create Supabase Cloud Database Project
1. Log in to [Supabase](https://supabase.com) and click **"New Project"**.
2. Name your project (e.g. `first-fest-2026`), choose your preferred region, and set a strong database password.
3. Once the project is provisioned, go to **Project Settings -> API**:
   - Copy the **Project URL** (`https://<project-ref>.supabase.co`)
   - Copy the **`anon` `public` key**
   - Copy the **`service_role` `secret` key** *(Keep this private!)*

### Step 2: Execute Database Schema
1. In the Supabase Dashboard, click on **SQL Editor** in the left sidebar.
2. Open [`supabase/schema.sql`](file:///C:/Users/DELL/.gemini/antigravity/scratch/first-fest-2026/supabase/schema.sql), paste the entire script into the editor, and click **Run**.
3. This creates:
   - Unique sequence: `registration_id_seq` starting at 1
   - Function: `generate_registration_id()` generating `FF2026-0001`, `FF2026-0002`...
   - Table: `public.registrations`
   - Table: `public.team_members`
   - Trigger: `update_updated_at_column()`
   - Optimized indexes for search and filtering
   - Row Level Security (RLS) policies allowing public registration inserts and authenticated admin management.

### Step 3: Configure Supabase Auth for Admin
1. In the Supabase Dashboard, go to **Authentication -> Users**.
2. Click **"Add User"** -> **"Create User"**.
3. Enter your event admin email (e.g., `admin@firstfest2026.com`) and choose a secure password.
4. Set **Auto Confirm User?** to `Checked / Yes`.

### Step 4: Configure Local Environment Variables
Create `.env` inside `backend/` and `frontend/` (or in project root):

```env
# Supabase Cloud Project Settings
SUPABASE_URL=https://<your-project-ref>.supabase.co
SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
SUPABASE_SERVICE_ROLE_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

# Backend Port
PORT=5000
NODE_ENV=production

# Frontend Client Environment (Vite)
VITE_SUPABASE_URL=https://<your-project-ref>.supabase.co
VITE_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
VITE_API_BASE_URL=/api
```

### Step 5: Run & Test Locally
```bash
# 1. Run automated validation test suite
node test/test_all_rules.js

# 2. Start Backend API
npm run dev --prefix backend

# 3. In another terminal, start Frontend Vite Server
npm run dev --prefix frontend
```
- Open `http://localhost:5173` to register a participant.
- Open `http://localhost:5173/admin` to log in as administrator and monitor entries.

### Step 6: Deploy to Vercel
1. Install Vercel CLI or import your GitHub repository into [Vercel Dashboard](https://vercel.com):
   ```bash
   vercel
   ```
2. In the Vercel Project Settings, add the **Environment Variables**:
   - `SUPABASE_URL`
   - `SUPABASE_ANON_KEY`
   - `SUPABASE_SERVICE_ROLE_KEY`
   - `VITE_SUPABASE_URL`
   - `VITE_SUPABASE_ANON_KEY`
3. Click **Deploy**. Vercel will build both the frontend and serverless API according to `vercel.json`.
4. Your production public link will be live at:
   `https://first-fest-2026.vercel.app`

---

## 🔒 Security Best Practices Implemented

1. **Secret Protection**: `SUPABASE_SERVICE_ROLE_KEY` is strictly used on the backend and NEVER exposed in frontend code.
2. **Rate Limiting**: Express rate limiter protects the `/api/register` endpoint against spam or DDoS attacks.
3. **HTTP Security Headers**: Powered by `helmet` to mitigate clickjacking and XSS.
4. **Input Sanitization & Server Validation**: Strict regex and schema validation protects against malformed inputs and parameter tampering.
5. **SQL Injection Protection**: Parameterized queries through `@supabase/supabase-js` client prevent SQL injection.
6. **Row Level Security (RLS)**: Enforced directly on PostgreSQL tables in Supabase Cloud.

---

## 📱 WhatsApp Sharing Preview

When you send the registration URL in WhatsApp or social groups, it renders a styled card:
- **Title**: FIRST FEST 2026 - Official Registration
- **Description**: Register now for FIRST FEST 2026. Technical & Non-Technical competitions with participants from premier colleges.
- **Theme Color**: `#0b0f19`
