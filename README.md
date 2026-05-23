<p align="center">
  <img src="https://img.shields.io/badge/Medi--Guard-AI-00d4ff?style=for-the-badge&logo=shield&logoColor=white" alt="Medi-Guard AI" />
  <img src="https://img.shields.io/badge/Neuro--Symbolic-Engine-7c3aed?style=for-the-badge" alt="Neuro-Symbolic" />
  <img src="https://img.shields.io/badge/version-2.0.0-10b981?style=for-the-badge" alt="Version 2.0.0" />
</p>

<h1 align="center">🛡️ Medi-Guard AI</h1>
<h3 align="center">Neuro-Symbolic Medical Prior Authorization System</h3>

<p align="center">
  An AI-powered prior authorization engine that combines <strong>neural NLP extraction</strong> (Google Gemini) with <strong>deterministic symbolic rule evaluation</strong> to deliver auditable, explainable, and policy-compliant medical claim adjudication.
</p>

---

## 🧠 Architecture Overview

```
                        ┌──────────────────────────────────────────┐
                        │           Medi-Guard AI v2.0             │
                        └──────────────────┬───────────────────────┘
                                           │
            ┌──────────────────────────────┼──────────────────────────────┐
            │                              │                              │
   ┌────────▼────────┐          ┌─────────▼─────────┐          ┌────────▼────────┐
   │   Next.js 16    │          │   FastAPI Backend  │          │   SQLite DB     │
   │   Frontend      │  ◄────► │   (Python 3.11+)   │  ◄────► │  medi_guard.db  │
   │   React 19      │  REST   │   JWT + RBAC       │  ORM    │  SQLAlchemy     │
   └─────────────────┘         └─────────┬──────────┘         └─────────────────┘
                                         │
                          ┌──────────────┼──────────────┐
                          │              │              │
                 ┌────────▼──────┐  ┌───▼────────┐  ┌──▼──────────────┐
                 │ 🧬 NEURAL     │  │ ⚙️ SYMBOLIC │  │ 🔐 BIOMETRIC    │
                 │ LLM Extractor │  │ Policy     │  │ VLM Face Auth   │
                 │ (Gemini 2.0)  │  │ Evaluator  │  │ (Gemini 2.5)    │
                 └───────────────┘  └────────────┘  └─────────────────┘
```

The **Neuro-Symbolic Pipeline** is the heart of Medi-Guard:

1. **Neural Layer** (`LLMExtractor`) — Uses Google Gemini to extract structured medical entities (diagnoses, CPT/ICD-10 codes, symptoms, treatments) from unstructured clinical notes.
2. **Symbolic Layer** (`PolicyEvaluator`) — Deterministically evaluates extracted entities against JSON-defined insurance policy rules. Zero LLM involvement — pure Python logic that produces auditable, explainable decisions.

> **Why Neuro-Symbolic?** LLMs are powerful at understanding language but can hallucinate. The symbolic layer acts as a guardrail, ensuring every decision is traceable to a concrete policy rule.

---

## 🏗️ Project Structure

```
medi/
├── backend/
│   ├── main.py                  # FastAPI application — all API endpoints
│   ├── database.py              # SQLAlchemy models (User, Submission, Bill, Report)
│   ├── requirements.txt         # Python dependencies
│   ├── passenger_wsgi.py        # Production WSGI adapter (cPanel/AquaHost)
│   ├── .env                     # Environment config (API keys, JWT secret, CORS)
│   └── engine/
│       ├── __init__.py
│       ├── llm_extractor.py     # 🧬 Neural Layer — Gemini-powered entity extraction
│       ├── policy_evaluator.py  # ⚙️ Symbolic Layer — deterministic rule engine
│       └── policies/
│           ├── starhealth_spinal_fusion.json
│           ├── apollohealth_knee_replacement.json
│           └── maxbupa_lumbar_mri.json
│
├── frontend/
│   ├── package.json
│   ├── next.config.ts
│   ├── src/app/
│   │   ├── page.tsx             # Landing page — cinematic hero with 3D intelligence core
│   │   ├── layout.tsx           # Root layout
│   │   ├── globals.css          # Global styles
│   │   ├── lib/
│   │   │   └── config.ts        # API base URL config (env-aware)
│   │   ├── auth/
│   │   │   └── login/
│   │   │       └── page.tsx     # Multi-role login with biometric face scan
│   │   ├── dashboard/
│   │   │   ├── doctor/          # Doctor portal — submit prior auth requests
│   │   │   ├── insurer/         # Insurer portal — review & override claims
│   │   │   ├── patient/         # Patient portal — view submissions & records
│   │   │   ├── hospital/        # Hospital portal — bills, reports, prescriptions
│   │   │   └── pharma/          # Pharmacy portal — drug adjudication
│   │   ├── simulator/
│   │   │   └── page.tsx         # Interactive engine demo/simulator
│   │   └── components/
│   │       └── 3d/
│   │           ├── IntelligenceCore.tsx  # Three.js 3D neural orb visualization
│   │           ├── DecisionVault.tsx     # 3D decision visualization component
│   │           └── AudioEngine.ts       # Ambient audio engine
│   └── public/                  # Static assets
│
└── README.md
```

---

## ✨ Features

### 🔬 Neuro-Symbolic AI Engine
- **LLM-Powered Extraction** — Converts unstructured clinical notes into structured JSON with diagnosis, CPT/ICD-10 codes, symptoms, treatment history, and confidence scoring
- **Deterministic Policy Evaluation** — JSON-defined insurance rules evaluated with full audit trails
- **Multi-Operator Rule Engine** — Supports `gte`, `lte`, `contains_any`, `in_list`, `starts_with_any` operators
- **Three-Tier Decision System** — `SUBMISSION_READY`, `NEEDS_MORE_EVIDENCE`, `REJECTED`

### 👥 Multi-Role Portal System
| Role | Capabilities |
|------|-------------|
| **Doctor** | Submit prior authorization requests, search patients, view submissions |
| **Insurer** | Review claims, override decisions (approve/deny), adjudicate prescriptions |
| **Patient** | View own submissions, medical records, bills, and authorization status |
| **Hospital** | Submit bills, reports, discharge summaries, manage patient records |
| **Pharmacy** | Submit prescriptions, drug-related authorization requests |

### 🔐 Security & Authentication
- **JWT-Based Auth** — 24-hour tokens with role-based access control (RBAC)
- **Biometric Face Verification** — Real-time webcam liveness detection powered by Gemini 2.5 Flash VLM
- **Password Hashing** — bcrypt with 72-char truncation
- **CORS Protection** — Environment-configurable allowed origins

### 🎨 Premium UI/UX
- **3D Intelligence Core** — Interactive Three.js neural orb on the landing page (React Three Fiber)
- **Cinematic Login Flow** — Multi-step biometric scan with grid overlays and sweep animations
- **Glassmorphism Design** — Premium dark-mode interface with blur effects and gradients
- **Framer Motion + GSAP** — Smooth page transitions and micro-animations
- **Five Role-Specific Dashboards** — Tailored experiences for each user type

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** ≥ 18 & **npm** ≥ 9
- **Python** ≥ 3.11
- **Google API Key** — [Get one from Google AI Studio](https://aistudio.google.com/apikey) (for Gemini models)

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/medi-guard.git
cd medi-guard
```

### 2. Backend Setup

```bash
cd backend

# Create virtual environment
python -m venv venv
source venv/bin/activate        # Linux/macOS
venv\Scripts\activate           # Windows

# Install dependencies
pip install -r requirements.txt

# Configure environment
cp .env.example .env
# Edit .env and set:
#   GOOGLE_API_KEY=your-api-key
#   JWT_SECRET=<generate with: python -c "import secrets; print(secrets.token_hex(32))">
#   ALLOWED_ORIGINS=http://localhost:3000

# Start the server
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

The API will be running at `http://localhost:8000`. Interactive docs available at `http://localhost:8000/docs`.

### 3. Frontend Setup

```bash
cd frontend

# Install dependencies
npm install

# Configure environment
echo "NEXT_PUBLIC_API_URL=http://localhost:8000" > .env.local

# Start dev server
npm run dev
```

The frontend will be running at `http://localhost:3000`.

---

## 🔌 API Reference

### Authentication
| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/auth/register` | Register a new user (any role) |
| `POST` | `/api/auth/login` | Login with credentials + optional biometric |
| `POST` | `/api/patients/register` | Register a patient from any portal |

### Core Engine
| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/submissions/evaluate` | Run the Neuro-Symbolic pipeline on clinical notes |
| `GET` | `/api/payers` | List all available insurance policies |
| `GET` | `/api/dashboard` | Role-scoped submission dashboard |

### Submissions
| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/submissions/{id}` | Get full submission details |
| `POST` | `/api/submissions/{id}/override` | Insurer manual override |

### Patient Records
| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/patients/search?code=PT-XXXX` | Search patient by code |
| `POST` | `/api/patients/{code}/history` | Add/update medical history |
| `POST` | `/api/patients/{code}/bills` | Submit a financial bill |
| `POST` | `/api/patients/{code}/reports` | Submit a medical report |
| `GET` | `/api/patients/{code}/records` | Get all patient records |
| `GET` | `/api/patients/{code}/prescriptions` | Get all prescriptions |
| `POST` | `/api/patients/{code}/adjudicate` | Adjudicate a prescription |

---

## ⚙️ Insurance Policy Configuration

Policies are defined as JSON files in `backend/engine/policies/`. Each policy specifies:

```json
{
  "payer_id": "starhealth_spinal_fusion",
  "payer_name": "Star Health Insurance",
  "procedure_name": "Lumbar Spinal Fusion",
  "description": "Prior authorization for lumbar spinal fusion surgery",
  "rules": [
    {
      "rule_id": "R1",
      "name": "Minimum Symptom Duration",
      "operator": "gte",
      "field": "symptom_duration_weeks",
      "threshold": 12,
      "on_fail": "NEEDS_MORE_EVIDENCE"
    }
  ]
}
```

**Available Operators:**

| Operator | Description | Example Use |
|----------|-------------|-------------|
| `gte` | Greater than or equal | Minimum symptom duration ≥ 12 weeks |
| `lte` | Less than or equal | BMI ≤ 40 |
| `contains_any` | List contains any keyword | Conservative treatments include "physical therapy" |
| `in_list` | Exact match in allowed list | CPT code in `["22612", "22630"]` |
| `starts_with_any` | String prefix match | ICD-10 starts with `"M51"` or `"M47"` |

### Currently Loaded Policies
- **Star Health** — Lumbar Spinal Fusion
- **Apollo Health** — Knee Replacement
- **Max Bupa** — Lumbar MRI

> To add a new policy, create a JSON file in `backend/engine/policies/` following the schema above. It will be auto-detected on server restart.

---

## 🗄️ Database Schema

| Table | Description |
|-------|-------------|
| `users` | All users (doctor, insurer, patient, hospital, pharma) with role-based fields |
| `medical_history` | Background medical context per patient |
| `submissions` | Prior authorization submissions with extracted data & audit trails |
| `bills` | Financial bills submitted by hospitals/pharmacies |
| `reports` | Medical reports, lab results, prescriptions |

Patient codes follow the format `PT-XXXXXX` (e.g., `PT-B4X9Q1`) — auto-generated, unique, and human-readable.

---

## 🛠️ Tech Stack

### Backend
| Technology | Purpose |
|-----------|---------|
| **FastAPI** | High-performance async Python web framework |
| **SQLAlchemy** | ORM for database modeling and queries |
| **SQLite** | Embedded database (production-ready with PostgreSQL swap) |
| **Google Gemini 2.0 Flash** | Neural entity extraction from clinical text |
| **Google Gemini 2.5 Flash** | Vision-language model for biometric face verification |
| **python-jose** | JWT token creation and validation |
| **bcrypt** | Secure password hashing |

### Frontend
| Technology | Purpose |
|-----------|---------|
| **Next.js 16** | React framework with App Router and static export |
| **React 19** | UI component library |
| **Three.js / React Three Fiber** | 3D neural orb visualization |
| **Framer Motion** | Page transitions and layout animations |
| **GSAP** | High-performance micro-animations |
| **Tailwind CSS 4** | Utility-first styling |
| **Lucide React** | Premium icon library |

---

## 🌐 Production Deployment

The project is configured for deployment at **[mediguard.site](https://mediguard.site)** with:

- **Frontend**: Static export via `next build` → served as static files
- **Backend**: Runs via `passenger_wsgi.py` on cPanel/AquaHost at `api.mediguard.site`
- **CORS**: Pre-configured for `mediguard.site` and `www.mediguard.site`

```bash
# Build frontend for production
cd frontend
npm run build

# The 'out/' directory contains the static export
```

---

## 📄 Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `GOOGLE_API_KEY` | ✅ | Google AI API key for Gemini models |
| `JWT_SECRET` | ✅ | Secret key for JWT token signing (64-char hex recommended) |
| `ALLOWED_ORIGINS` | ⬚ | Comma-separated list of allowed CORS origins |
| `DATABASE_URL` | ⬚ | Database connection string (defaults to SQLite) |
| `NEXT_PUBLIC_API_URL` | ✅ | Backend API URL for the frontend |

---

## 📜 License

This project is proprietary. All rights reserved.

---

<p align="center">
  Built with 🧠 Neuro-Symbolic AI &nbsp;•&nbsp; Powered by Google Gemini &nbsp;•&nbsp; Secured with Biometric VLM
</p>
