# Construction Q\&A + Discrepancy Checker — MVP.

## Overview

This project is an AI assistant for the construction industry that unifies data scattered across Procore, Autodesk Docs, Gmail, and local file systems. It enables project managers and coordinators to:

* **Ask questions** and get instant, cited answers from project documents.
* **Draft RFIs/Submittals** with auto-filled references and attachments.
* **Catch discrepancies** between specs, drawings, and submittals before they lead to delays or rework.

The system uses Retrieval-Augmented Generation (RAG) with embeddings and semantic search to ensure answers are contextual, grounded, and cited.

---

## Features (MVP)

* **Integration Layer**: Connect Procore, Autodesk Docs, Gmail (optional), and local uploads.
* **Ingestion Pipeline**: Fetch, normalize, OCR, chunk, embed, and store documents with metadata.
* **Q\&A System**: Hybrid search + LLM generation with inline citations.
* **RFI/Submittal Drafting**: Auto-generate structured drafts with references and export to PDF.
* **Discrepancy Checker**: Identify mismatches across drawings/specs/submittals and surface them in a UI.

---

## Tech Stack

* **Language**: Python 3.11
* **Framework**: FastAPI
* **RAG Framework**: LangChain or LlamaIndex
* **Embeddings + Vector DB**: OpenAI (text-embedding-3-large) + Pinecone
* **Database**: PostgreSQL 15
* **Storage**: AWS S3 or local file system
* **Connectors**: Procore API, Autodesk Docs API, Gmail API
* **Infra**: Docker + Docker Compose (local), Railway/Render/AWS EC2 (prototype deployment)
* **Frontend**: React/Next.js + Tailwind (basic UI for MVP)

---

## Architecture

```
[User Web UI]
   │
   ▼
[FastAPI Backend]
   ├── Integrations Layer (Procore, Autodesk Docs, Gmail, Local Uploads)
   ├── Ingestion Pipeline (OCR, extraction, chunking, embedding)
   ├── RAG Orchestrator (LangChain/LlamaIndex)
   ├── RFI/Submittal Draft Service
   ├── Discrepancy Checker
   ├── Storage: PostgreSQL + S3 + Pinecone
   └── Observability: logs, metrics, audit trail
```

---

## Repo Structure

```
construction-brain/
  backend/
    app/
      api/
      core/
      services/
      models/
      schemas/
      workers/
    tests/
    alembic/
    Dockerfile
    docker-compose.yml
  web/
    src/
  ops/
    terraform/
  docs/
    eval/
```

---

## Setup Instructions

### Prerequisites

* Python 3.11+
* Docker & Docker Compose
* PostgreSQL 15 (local or via Docker)
* Pinecone account (for vector DB)
* OpenAI API key

### Installation

```bash
# Clone repo
$ git clone https://github.com/<your-org>/construction-brain.git
$ cd construction-brain

# Backend setup
$ cd backend
$ python -m venv venv
$ source venv/bin/activate
$ pip install -r requirements.txt

# Run with Docker Compose
$ docker-compose up --build
```

Backend should now be running at `http://localhost:8000`.

---

## API Endpoints (MVP)

* `POST /projects` – create project
* `POST /connect/procore` – connect Procore account
* `POST /connect/autodesk` – connect Autodesk Docs
* `POST /files/upload` – upload and ingest files
* `POST /ask` – ask a question → returns {answer, citations\[]}
* `POST /rfi/draft` – generate RFI draft
* `GET /discrepancies` – list discrepancies

---

## Example Usage

```bash
curl -X POST http://localhost:8000/ask \
  -H "Content-Type: application/json" \
  -d '{"project_id": "123", "question": "What is the fire rating for corridor doors?"}'
```

**Response:**

```json
{
  "answer": "Corridor doors must have a 90-minute fire rating.",
  "citations": ["Spec 08 10 00 §2.3.1 p.45"]
}
```

---

## Milestones (6 Weeks)

* **Week 1**: FastAPI skeleton + local upload ingestion → /ask
* **Week 2**: Procore connector + ingest RFIs/Submittals
* **Week 3**: Autodesk Docs connector + section-aware chunking
* **Week 4**: RFI/Submittal drafting with PDF export
* **Week 5**: Discrepancy checker for doors, ducts, concrete PSI
* **Week 6**: Evaluation with 40-question gold set + demo polish

---

## Evaluation Metrics

* Groundedness (% answers with correct citations)
* Hallucination rate
* Latency (P50/P90)
* Human-rated clarity and completeness

---

## Contributing

Contributions are welcome! Please fork the repo, create a feature branch, and submit a PR.

---

## License

MIT License (update as needed).
