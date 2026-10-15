# AI Customer Support Chatbot

A RAG-powered support bot. It answers customer questions using only your own documents, and shows which chunks it used.

**Stack:** Python, FastAPI, ChromaDB (vector search), Ollama (free, local) or Claude API.

## How it works

```
docs (data/*.md) -> chunk -> embed -> Chroma
question -> retrieve top 3 chunks -> LLM answers from them -> reply + sources
```

## Setup

```bash
python -m venv venv
venv\Scripts\activate          # Mac/Linux: source venv/bin/activate
pip install -r requirements.txt
copy .env.example .env         # Mac/Linux: cp .env.example .env
```

**Option A: Ollama (free, no key)**
1. Install Ollama from https://ollama.com
2. Run `ollama pull llama3.2`

**Option B: Claude API**
Set `LLM_PROVIDER=claude` and `ANTHROPIC_API_KEY=...` in `.env`.

## Run

```bash
python -m app.ingest
uvicorn app.main:app --reload
```

Open http://127.0.0.1:8000/docs, try `POST /chat` with:

```json
{"messages": [{"role": "user", "content": "Can I return a sale item?"}]}
```

## Project structure

```
app/ingest.py   load docs, chunk, store embeddings
app/rag.py      retrieve + build the grounded prompt
app/llm.py      Ollama / Claude switch
app/main.py     FastAPI /chat endpoint
data/faq.md     your knowledge base (add more .md files)
```

## Roadmap
- [ ] Web chat UI with streaming
- [ ] Chat memory
- [ ] Human handoff + logging
- [ ] Answer quality evaluation
- [ ] Deploy
