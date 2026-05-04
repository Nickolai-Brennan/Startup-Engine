


---

#🚀 PreLauncher — Startup Framework Template

> A reusable foundation for launching full-stack apps with consistent architecture, standards, and workflows.




---

📌 Overview

PreLauncher is a starter repository + system framework designed to:

Standardize project setup

Enforce architecture consistency

Accelerate MVP development

Integrate AI-assisted workflows (Copilot, ChatGPT, etc.)

Connect frontend, backend, and database seamlessly



---

🧱 Core Stack

Frontend

React

TanStack (Query, Router, Table)

TailwindCSS


Backend

FastAPI (Python)

Pydantic

SQLAlchemy


Database

PostgreSQL

Schemas:

app_public

golf

sports_betting

models

analytics




API Layer

GraphQL (user/app data layer)

REST (internal services via FastAPI)


DevOps

Docker

GitHub Actions (CI/CD)

Caddy / Nginx (optional)



---

📂 Project Structure

prelauncher/
│
├── frontend/              # React + UI system
├── backend/               # FastAPI services
├── graphql/               # Schema + resolvers
├── database/              # SQL, migrations, seeds
├── docker/                # Container configs
├── docs/                  # Project documentation
│
├── .github/
│   ├── copilot-instructions.md
│   ├── agents/
│   └── workflows/
│
├── xXx_PreLauncher/       # Framework + rules (THIS SYSTEM)
├── xXx_Lab/               # Actions, agents, prompts
├── xXx_Library/           # Docs, references, cheatsheets
├── xXx_BlueprintZ/        # UI + product templates
│
└── README.md


---

⚙️ Installation

1. Clone Repo

git clone https://github.com/your-org/prelauncher.git
cd prelauncher


---

2. Environment Setup

Create .env file:

POSTGRES_DB=caddy_stats
POSTGRES_USER=postgres
POSTGRES_PASSWORD=password

FASTAPI_ENV=development
GRAPHQL_ENDPOINT=http://localhost:8000/graphql


---

3. Run with Docker

docker-compose up --build


---

4. Run Services Individually (Optional)

Backend

cd backend
pip install -r requirements.txt
uvicorn main:app --reload

Frontend

cd frontend
npm install
npm run dev


---

🔗 System Architecture

Frontend (React + TanStack)
        ↓
GraphQL API Layer
        ↓
FastAPI Services (Business Logic)
        ↓
PostgreSQL (Multi-Schema DB)


---

🧠 Development Philosophy

Build modular components

Keep schemas separated by domain

Use GraphQL for UI-driven data

Use FastAPI for logic + processing

Prefer reusable templates over one-offs



---

🤖 AI + Automation Layer

This repo is designed to work with:

GitHub Copilot (repo-wide agents)

ChatGPT (prompt workflows)

Custom Agents (in /xXx_Lab/agents)


Capabilities

Code cleanup & formatting

Architecture validation

Schema enforcement

UI component generation

Documentation generation



---

📊 Database Strategy

Primary DB: caddy_stats

Schema Responsibilities

Schema	Purpose

app_public	Users, auth, site data
golf	Courses, events, player stats
sports_betting	Odds, bets, books
models	Prediction models
analytics	Derived metrics + logs



---

🧩 Modules Included

Blog System (Magazine Layout Ready)

WeatherTrax (Weather + course conditions)

Data Models (Golf + Betting)

UI Component Library (cards, tables, dashboards)



---

🧱 Build Workflow

1. Define idea (PreLauncher template)


2. Structure project (folders + schemas)


3. Spin up services


4. Build UI components


5. Connect GraphQL


6. Connect DB


7. Add automation (agents/prompts)


8. Deploy




---

📁 Internal Systems

xXx_PreLauncher

Core rules

Architecture standards

Startup template


xXx_Lab

Agents

Prompts

Actions

Experimental tools


xXx_Library

Docs

Cheatsheets

References


xXx_BlueprintZ

UI templates

Layout systems

Design blocks



---

🔐 Best Practices

Never mix schema responsibilities

Keep API contracts consistent

Reuse components aggressively

Document everything in /docs

Validate data at every layer



---

🚀 Future Additions

MCP Task Orchestrator

Automated schema migrations

Visual dashboard builder

AI-powered content generator

Plugin system for microservices



---

📬 Contribution

1. Fork repo


2. Create feature branch


3. Submit PR


4. Follow architecture standards




---

🧾 License

MIT License


---

If you want next step:

I can generate the actual repo file structure (folders + starter files)

or build docker-compose + base FastAPI + GraphQL + React starter code wired together
