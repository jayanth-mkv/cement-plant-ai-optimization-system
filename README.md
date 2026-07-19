<p align="center">
  <img src="docs/assets/icon.svg" width="132" alt="Cement Plant AI operations icon">
</p>

<h1 align="center">Cement Plant AI Optimization</h1>

<p align="center"><strong>An AI-assisted operations prototype for monitoring cement production, exploring cross-process trade-offs, and presenting optimization recommendations in real time.</strong></p>

<p align="center">
  <a href="frontend/package.json"><img alt="Next.js 15" src="https://img.shields.io/badge/Next.js-15-241E16?logo=next.js&logoColor=white"></a>
  <a href="server/pyproject.toml"><img alt="Python 3.13 and FastAPI" src="https://img.shields.io/badge/FastAPI-Python_3.13-665441?logo=fastapi&logoColor=white"></a>
  <a href="LICENSE"><img alt="MIT license" src="https://img.shields.io/badge/license-MIT-241E16"></a>
</p>

<p align="center">
  <a href="#what-is-cement-plant-ai-optimization">What is it?</a> ·
  <a href="#capabilities">Capabilities</a> ·
  <a href="#walkthrough">Walkthrough</a> ·
  <a href="#architecture">Architecture</a> ·
  <a href="#quick-start">Quick start</a> ·
  <a href="#known-limitations">Limitations</a>
</p>

<p align="center">
  <img src="docs/assets/demos/cement-ai-preview.gif" width="92%" alt="Animated Cement Plant AI demo showing executive monitoring, agent analysis, AI insights, and fuel optimization">
</p>

## What is Cement Plant AI Optimization?

Cement Plant AI Optimization is an end-to-end hackathon prototype for exploring plant operations, cross-process trade-offs, and AI-assisted recommendations from one operator-facing workspace.

It combines a Next.js dashboard, FastAPI service, LangGraph/Gemini decision agent, and PostgreSQL data access through MCP. The experience is designed for scenario exploration and qualified operator review, not autonomous plant control.

> **Project status:** Hackathon prototype built for the Google Gen AI Exchange Hackathon 2025. The repository includes the dashboard, FastAPI service, LangGraph agent, WebSocket flows, and PostgreSQL/MCP integration. KPI improvements shown in the product are simulated targets for demonstration; they are not production-plant measurements.

## Capabilities

| Surface | Implemented capability |
| --- | --- |
| Operations dashboard | Live KPIs, process views, optimization controls, alerts, and trend visualizations |
| Backend API | FastAPI routes for plant data, analytics, AI recommendations, and WebSocket streams |
| Decision agent | LangGraph workflow using Gemini and database tools exposed through MCP |
| Data layer | PostgreSQL/Supabase access for operational and historical data |
| Process coverage | Raw materials, kiln control, quality, alternative fuels, maintenance, and utilities |

## Walkthrough

A typical demo flow is:

1. Stream plant data into the dashboard.
2. Inspect energy, quality, cost, and maintenance signals across process areas.
3. Ask the agent to analyze a plant condition or optimization goal.
4. Review the recommendation, supporting metrics, and projected operational impact.

The architecture below is the canonical overview of the current prototype.

## Architecture

<p align="center">
  <img src="docs/assets/readme-architecture.svg" width="92%" alt="Cement Plant AI prototype flow from plant data through APIs and agent reasoning to operator review">
</p>

### Runtime services

| Service | Default address | Purpose |
| --- | --- | --- |
| Frontend | `http://localhost:3000` | Operations dashboard |
| Backend | `http://localhost:8000` | REST API, WebSockets, and OpenAPI docs at `/docs` |
| PostgreSQL MCP | `http://localhost:8080` | Database tools for the agent |
| LangGraph dev server | `http://localhost:2024` | Agent development and inspection |

## Quick start

### Prerequisites

- Node.js 20+
- Python 3.13
- [uv](https://docs.astral.sh/uv/)
- PostgreSQL or a Supabase project
- Gemini API key
- Windows PowerShell for the included multi-service helper

### 1. Install

```powershell
git clone https://github.com/jayanth-mkv/cement-plant-ai-optimization-system.git
cd cement-plant-ai-optimization-system

cd frontend
npm install
cd ..\server
uv sync
cd ..
```

### 2. Configure

Copy `server/.env.example` to `server/.env`, then provide your own credentials:

```dotenv
SUPABASE_URL=https://your-project-ref.supabase.co
SUPABASE_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key
DATABASE_URL=postgresql://user:password@host:5432/database
API_HOST=0.0.0.0
API_PORT=8000
DEBUG=True
SCHEDULER_TIMEZONE=UTC
```

The agent also needs a database connection and model credential in `server/cement_agent/.env` or its runtime environment:

```dotenv
DATABASE_URI=postgresql://user:password@host:5432/database
GEMINI_API_KEY=your_gemini_api_key
GOOGLE_API_KEY=your_gemini_api_key
LANGSMITH_PROJECT=cement-plant-optimization
```

Never commit real credentials. The examples above are placeholders.

### 3. Run

```powershell
.\dev.ps1 dev
```

The helper opens the frontend, backend, agent, and MCP service in separate PowerShell windows. Start one service at a time when debugging:

```powershell
.\dev.ps1 mcp
.\dev.ps1 back
.\dev.ps1 agent
.\dev.ps1 front
```

## Manual development commands

```powershell
# Backend
cd server
uv run main.py

# PostgreSQL MCP — run before the agent
uv run postgres-mcp --sse-port 8080 --transport sse --access-mode unrestricted $env:DATABASE_URI

# Agent
cd cement_agent
uv run langgraph dev

# Frontend
cd ..\..\frontend
npm run dev
```

## Prototype targets

These values describe the targets modeled in the hackathon experience, not independently validated production results:

| Area | Demonstrated target |
| --- | --- |
| Energy | Up to 15% modeled reduction |
| Alternative fuel | Up to 40% modeled thermal substitution |
| Quality inspection | 99%+ target for the planned vision-assisted workflow |
| Unplanned maintenance | 20% modeled reduction |

## Technology

- **Frontend:** Next.js 15, React 19, TypeScript, Tailwind CSS, Radix UI, Recharts
- **Backend:** FastAPI, Pydantic, asyncpg, APScheduler, WebSockets
- **Agent layer:** Gemini, LangGraph, LangChain MCP adapters, PostgreSQL MCP
- **Data:** PostgreSQL and Supabase Realtime
- **Tooling:** uv, npm, Docker, ESLint

## Repository map

```text
.
├── frontend/              Next.js dashboard
├── server/                FastAPI service and data layer
│   └── cement_agent/      LangGraph agent and MCP tools
├── dev.ps1                Windows multi-service launcher
└── todo.md                Project backlog
```

## Known limitations

- The data-ingestion workflows used for the demonstration are not included.
- Computer-vision quality inspection and production-grade Supabase policies remain planned work.
- The prototype is not a replacement for a plant DCS/SCADA safety system.
- Recommendations must be reviewed by qualified operators before any real-world use.

## Contributing

Focused issues and pull requests are welcome. Please describe the process area, expected behavior, and how the change can be tested.

## License

[MIT](LICENSE)
