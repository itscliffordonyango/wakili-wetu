# Wakili Wetu — Legal Assistant (demo)

*A lightweight Flask-based legal research assistant with support for multiple LLM providers and a simple offline legal engine for demos.*

## What this repo contains

- `backend/` — Flask API, DB models, AI integration, and a small TF‑IDF based local legal engine.
- `static/wakili_wetu.html` — single-page frontend UI used by the demo.
- `backend/legal_docs/` — place legal `.txt` files here for the local engine to index.

## Quick start

1. Create and activate a Python virtual environment (recommended):

```bash
python3 -m venv .venv
source .venv/bin/activate
```

2. Install dependencies:

```bash
pip install -r backend/requirements.txt
```

3. Configure an AI provider (choose one):

- Local demo engine (recommended for pitch/demo):

```bash
export AI_PROVIDER=local
```

- OpenAI (requires an API key and model you have access to):

```bash
export AI_PROVIDER=openai
export OPENAI_API_KEY="sk-..."
export OPENAI_MODEL="gpt-4.1-mini"
```

- Gemini (requires Google/Gemini key):

```bash
export AI_PROVIDER=gemini
export GEMINI_API_KEY="AIza..."
```

4. Run the app (from the repo root):

```bash
cd backend
python3 app.py
```

The server runs on `http://127.0.0.1:5000` by default.

## API (important endpoints)

- `POST /api/ai/analyze` — JSON `{ "text": "..." }` → returns `{ text, cites }`.
- `POST /api/ai/upload` — multipart file upload (file is saved and summarized).
- `POST /api/cases/analyze` — JSON `{ "query": "..." }` → runs a search + analysis and persists a `CaseAnalysis`.
- Auth routes live under `/api/auth` (register/login) and are used by the frontend.

## Local demo engine

The `local` provider uses `backend/services/local_legal_engine.py`. Drop plain `.txt` legal texts into `backend/legal_docs/` and the local engine will:
- Index and chunk documents with TF‑IDF
- Return short, human-readable explanations and a list of source filenames under `cites`

This mode is ideal for offline demos and pitching while external LLM access (OpenAI/Gemini) is unavailable or restricted.

## Notes & troubleshooting

- If you see fallback messages such as `[fallback] ...` it usually means the configured LLM provider could not be reached or the API key/model is not available. Use `AI_PROVIDER=local` to demo immediately.
- The app creates its SQLite DB under the `backend` folder; tables are auto-created on first run.

## Relevant files

- `backend/services/ai_engine.py` — provider dispatcher and integration logic.
- `backend/services/local_legal_engine.py` — offline TF‑IDF search + answer generator.
- `static/wakili_wetu.html` — front-end demo UI.

If you'd like, I can also add a short `docker-compose` or a tiny FastAPI inference shim for running a hosted model endpoint. Want that next?
