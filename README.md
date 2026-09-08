# NeuralMark

**An interactive, educational dashboard for understanding Hybrid Search.**

NeuralMark is a hands-on visualization tool that demystifies how modern search
engines combine *keyword matching* with *semantic understanding*. Instead of
treating hybrid search as a black box, NeuralMark exposes every step of the
pipeline — sparse scores, dense scores, and the fusion math that merges
them — so you can see exactly why a document ranks where it does.

---

## Purpose

Most production search systems (e-commerce, RAG pipelines, enterprise search)
don't rely on a single retrieval method. They blend two complementary
signals:

- **Sparse retrieval (BM25)** — fast, exact keyword matching. Great for
  rare terms, product codes, names — things embeddings tend to blur.
- **Dense retrieval (embeddings)** — captures semantic meaning. Great for
  paraphrases and conceptual similarity, even with zero keyword overlap.

NeuralMark lets you issue a query, watch both retrievers score every
document independently, and then tune an **alpha (α) slider** that controls
how much weight each method gets in the final fused ranking. It's built for
learning — every intermediate score is visible, not hidden behind an
API response.

## How It Works

1. **Ingestion** — A small demo corpus is indexed two ways: a `BM25Okapi`
   index (tokenized keyword statistics) and a dense vector index (sentence
   embeddings from `all-MiniLM-L6-v2`).
2. **Query time** — A user query is scored against both indexes
   independently, producing two ranked lists.
3. **Fusion (RRF)** — Instead of naively averaging incompatible score
   scales (BM25 scores and cosine similarities live in very different
   ranges), NeuralMark uses **Reciprocal Rank Fusion**:

   ```
   RRF_score(d) = Σ  1 / (k + rank_i(d))
   ```

   summed over each retrieval method `i`, where `rank_i(d)` is the
   document's position in that method's ranked list. The α slider
   controls a weighted variant of this fusion so you can lean toward
   pure keyword search, pure semantic search, or anywhere in between.
4. **Visualization** — The React dashboard shows per-document score
   breakdowns, rank movement between methods, and how the final fused
   order shifts live as you drag α.

## Tech Stack

| Layer      | Technology                                                  |
|------------|--------------------------------------------------------------|
| Backend    | Python 3.11, FastAPI, Uvicorn                                 |
| Algorithms | `rank_bm25`, `sentence-transformers` (all-MiniLM-L6-v2), scikit-learn (PCA for embedding visualization) |
| Frontend   | React (Vite), Tailwind CSS, Lucide Icons                      |
| DevOps     | Docker, Docker Compose                                        |

## Project Structure

```
neuralmark/
├── backend/
│   ├── main.py
│   ├── requirements.txt
│   └── Dockerfile
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   └── SearchDashboard.jsx
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── package.json
│   └── Dockerfile
├── docker-compose.yml
└── README.md
```

## Quick Start

### Prerequisites
- Docker & Docker Compose installed
- (Optional, for local dev without Docker) Python 3.11+ and Node 18+

### Run with Docker Compose

```bash
# From the project root
docker compose up --build
```

- Backend API available at **http://localhost:8000** (docs at `/docs`)
- Frontend dashboard available at **http://localhost:5173**

### Run locally without Docker

**Backend:**
```bash
cd backend
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate
pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```

**Frontend:**
```bash
cd frontend
npm install
npm run dev
```

## Roadmap
- [ ] Backend API skeleton + health check
- [ ] BM25 sparse index
- [ ] Dense embedding index (SentenceTransformers)
- [ ] Reciprocal Rank Fusion endpoint with tunable alpha
- [ ] React dashboard with alpha slider + rank inspection
- [ ] PCA-based embedding space visualization
- [ ] Dockerize backend + frontend, wire up Compose

## License
MIT (or your license of choice)****
