# BiomarkerIQ 
<img width="1638" height="977" alt="image" src="https://github.com/user-attachments/assets/35a566d6-c5c1-4221-8110-64d205dfeda5" />

## Architecture

```
Frontend (React + Vite + TypeScript + Tailwind)
        │  HTTP polling (every 3s)
        ▼
Backend (FastAPI + Python)
        │
        ▼
ResearchOrchestrator (Python coordinator)
        │
   ┌────┬────────┬──────────┬──────┬───────────┬──────┐
   ▼    ▼        ▼          ▼      ▼           ▼      ▼
Planner Target  Literature Struct Drug       Evidence Report
Agent   Intel.  Agent      Agent  Discovery  Scoring  Agent
        Agent                    Agent       Agent
  │      │         │         │       │          │       │
LLM  UniProt   PubMed/    PDB/   ChEMBL/    LLM     LLM
only  NCBI    Europe PMC  AF2   PubChem    reasoning compile
```

## Project Structure

```
biomarkeriq/
├── backend/
│   ├── .env.example
│   ├── requirements.txt
│   ├── config.py              ← loads GEMINI_API_KEY from .env only
│   ├── main.py                ← FastAPI app + CORS
│   ├── models/schemas.py      ← Pydantic models
│   ├── tools/                 ← API integration functions
│   │   ├── pubmed_tool.py     ← PubMed + Europe PMC
│   │   ├── uniprot_tool.py    ← UniProt + NCBI Gene
│   │   ├── pdb_tool.py        ← RCSB PDB + AlphaFold
│   │   ├── chembl_tool.py     ← ChEMBL bioactivity
│   │   └── pubchem_tool.py    ← PubChem compounds
│   ├── agents/                ← ADK LlmAgent wrappers
│   │   ├── base_agent.py      ← BaseAgent (ADK setup, JSON parsing)
│   │   ├── planner_agent.py   ← Query understanding + research plan
│   │   ├── target_intelligence_agent.py
│   │   ├── literature_agent.py
│   │   ├── structure_agent.py
│   │   ├── drug_discovery_agent.py
│   │   ├── evidence_scoring_agent.py
│   │   └── report_agent.py    ← Markdown report compiler
│   ├── services/
│   │   ├── job_service.py     ← In-memory job store (thread-safe)
│   │   └── report_service.py  ← MD→PDF conversion (weasyprint)
│   └── api/routes.py          ← FastAPI routes + background thread runner
│
└── frontend/
    ├── src/
    │   ├── types/index.ts
    │   ├── services/api.ts    ← Axios API client
    │   ├── hooks/useResearch.ts ← Polling hook (3s interval)
    │   ├── pages/
    │   │   ├── HomePage.tsx   ← Hero + search
    │   │   └── ResearchPage.tsx
    │   └── components/
    │       ├── SearchBar.tsx
    │       ├── AgentStatusPanel.tsx
    │       ├── ReportViewer.tsx
    │       ├── ExportButtons.tsx
    │       └── sections/      ← Individual report section cards
    └── index.css              ← Dark theme, glassmorphism, animations
```


