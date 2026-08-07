# 🤖 Conversational Data Analyst (Lapis AI)

[![Live Demo](https://img.shields.io/badge/Live_Demo-Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white)](https://chatbot-data-analyst2.vercel.app/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![React](https://img.shields.io/badge/Frontend-React_19_%7C_Vite_%7C_TypeScript-61DAFB?logo=react)](frontend/)
[![FastAPI](https://img.shields.io/badge/Backend-FastAPI_%7C_Python-009688?logo=fastapi)](backend/)
[![Supabase](https://img.shields.io/badge/Database-PostgreSQL_%7C_Supabase-3ECF8E?logo=supabase)](https://supabase.com)

🚀 **Live Deployment**: [https://chatbot-data-analyst2.vercel.app/](https://chatbot-data-analyst2.vercel.app/)

An end-to-end, production-grade **AI-Powered Conversational Data Analyst System**. This application empowers users to query databases using natural language, automatically generating safe SQL queries, executing them, generating natural language explanations, recommending interactive charts, and managing multi-session chat histories.

---


## 🌟 Key Features

- **🗣️ Natural Language to SQL Generation**: Translates plain text user prompts into precise PostgreSQL queries using LLMs (Groq Llama 3.3 70B / Google Gemini).
- **🛡️ Safe SQL Guardrails & RBAC**: Enforces strict read-only query execution, query timeouts, table denylists, and role-based access control (Admin vs. Standard User).
- **📊 Dynamic Data Visualizations**: Automatically analyzes SQL query results and recommends/renders interactive charts (Bar, Line, Pie, Area, Scatter) powered by Recharts.
- **💬 Multi-Session Chat & Persistence**: Supports persistent chat threads, auto-naming of chat sessions based on context, session renaming, and chat history backed by PostgreSQL.
- **📋 Admin Audit Trail & Query Logs**: Complete visibility for administrators including query execution times, success rates, system logs, and write action confirmations.
- **📥 Multi-Format Data Export**: Export query results and data insights seamlessly into **CSV**, **PDF**, and **PNG** chart images.
- **🧪 AI Evaluation & Benchmarking Framework**: Built-in benchmark suite to evaluate LLM SQL accuracy, execution correctness, and result set validity.
- **⚡ Vercel All-in-One Serverless Ready**: Configured for effortless full-stack deployment on Vercel with Python serverless functions and a Vite React frontend.

---

## 🏗️ Architecture & Pipeline Flow

The AI pipeline is designed as an isolated domain layer without HTTP framework locks:

```mermaid
graph TD
    User([User Question]) --> WebLayer[FastAPI / Web Route]
    WebLayer --> RBAC[RBAC & Safety Context Verification]
    RBAC --> PromptEng[Prompt Builder & Context Injector]
    PromptEng --> LLM[LLM Generator: Groq Llama 3.3 / Gemini]
    LLM --> Validator[SQL Guard & Read-Only Validator]
    Validator -- Safe SQL --> DB[(Supabase PostgreSQL)]
    Validator -- Invalid / Unsafe --> Clarify[Clarification / Error Response]
    DB --> Summarizer[Insight & Explanation Generator]
    DB --> ChartRec[Chart Recommender & Recharts Spec]
    Summarizer --> Response[Interactive UI Response]
    ChartRec --> Response
```

---

## 📁 Repository Structure

```
Chatbot-Data-Analyst/
├── api/                         # Vercel Serverless Entrypoint
│   └── index.py                 # Serverless WSGI wrapper for FastAPI
├── backend/                     # Backend Python Application
│   └── backend/
│       ├── api/                 # FastAPI routes (auth, ask, admin, evaluation)
│       └── ai/                  # AI Domain Layer
│           ├── llm/             # LLM provider clients (Groq, Gemini) & Orchestrator
│           ├── prompts/         # Task-specific prompt builders
│           ├── validators/      # SQL safety guards & RBAC checks
│           ├── rbac/            # Role & permission definitions
│           ├── chart/           # Visualization spec generation
│           ├── evaluation/      # LLM Benchmark & accuracy evaluation suite
│           └── utils/           # Supabase client, schema loader & DB query executors
├── frontend/                    # Frontend React Application
│   ├── src/
│   │   ├── components/          # Reusable UI elements, Sidebar & Visualizations
│   │   ├── pages/               # ChatPage, QueryLogs, Dashboard pages
│   │   ├── lib/                 # API Client, Export Utilities (PDF/CSV) & Zustand stores
│   │   └── types/               # TypeScript interfaces & types
│   ├── index.html               # Main HTML entrypoint
│   └── vite.config.ts           # Vite configuration with Tailwind CSS v4
├── package.json                 # Root Vercel build script
└── vercel.json                  # Vercel deployment configuration
```

---

## 🚀 Getting Started

### Prerequisites

- **Python**: 3.10 or higher
- **Node.js**: v18+ and npm
- **Database**: PostgreSQL (Supabase recommended)
- **API Key**: Groq API Key or Google Gemini API Key

---

### 1. Backend Setup

1. Navigate to the backend directory:
   ```bash
   cd backend
   ```

2. Create a virtual environment and activate it:
   ```bash
   python -m venv venv
   # On Windows:
   .\venv\Scripts\activate
   # On Linux/macOS:
   source venv/bin/activate
   ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

4. Create a `.env` file in `backend/backend/`:
   ```env
   # LLM Configuration
   GROQ_API_KEY=your_groq_api_key_here
   GROQ_MODEL=llama-3.3-70b-versatile
   
   # Database Credentials
   DATABASE_URL=postgresql://user:password@hostname:5432/dbname
   DATABASE_SCHEMA=public
   
   # App & Security
   APP_ENV=development
   ENABLE_SQL_VALIDATION=true
   MAX_QUERY_TIMEOUT=30
   ```

5. Run the FastAPI development server:
   ```bash
   uvicorn backend.api.main:app --reload --port 8000
   ```

---

### 2. Frontend Setup

1. Navigate to the frontend directory:
   ```bash
   cd frontend
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Start the Vite development server:
   ```bash
   npm run dev
   ```

4. Open your browser and navigate to `http://localhost:5173`.

---

## 🌐 Vercel Deployment

This project is pre-configured for full-stack serverless deployment on **Vercel**.

1. Connect your GitHub repository to Vercel.
2. Ensure Environment Variables (`GROQ_API_KEY`, `DATABASE_URL`, etc.) are configured in Vercel Project Settings.
3. Deploy! Vercel automatically uses `vercel.json` to build:
   - Frontend Vite bundle (`frontend/dist`)
   - Python API Serverless Function (`api/index.py`)

---

## 🔐 Security & Safety

- **Read-Only Enforcement**: Blocks `DROP`, `DELETE`, `INSERT`, `UPDATE`, `ALTER`, and `TRUNCATE` queries for standard chat endpoints.
- **RBAC Schema Guard**: Restricts table access based on user role context.
- **SQL Injection Safeguards**: Queries are parsed, sanitized, and validated prior to database execution.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
