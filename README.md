# NeuralMark
> An interactive, educational hybrid search engine dashboard demonstrating BM25, Dense Vector Embeddings, and Reciprocal Rank Fusion (RRF).

---

## Overview
NeuralMark provides a visual playground to understand how modern hybrid retrieval systems balance **sparse keyword search** and **dense semantic search**. By exposing the inner mechanics of Reciprocal Rank Fusion (RRF), users can adjust weighting controls ($\alpha$) in real-time and inspect rank list shifts.

---

## How It Works
1. **Sparse Search (BM25):** Calculates term frequency and inverse document frequency to score exact lexical matches.
2. **Dense Search (SentenceTransformers):** Encodes documents and queries into 384-dimensional vector space using `all-MiniLM-L6-v2` and computes cosine similarity.
3. **Reciprocal Rank Fusion (RRF):** Fuses rankings from both retrieval systems into a unified score using:

$$RRF\_Score(d) = \frac{\alpha}{k + r_{BM25}(d)} + \frac{1 - \alpha}{k + r_{Dense}(d)}$$

4. **Dimensionality Reduction (PCA):** Projects vector embeddings into 2D space for intuitive visualization on the React dashboard.

---

## Tech Stack
* **Backend:** Python 3.11, FastAPI, Uvicorn, `rank_bm25`, `sentence-transformers`, `scikit-learn`
* **Frontend:** React (Vite), Tailwind CSS, Lucide Icons
* **DevOps:** Docker, Docker Compose

---

## Quick Start

### Prerequisites
* [Docker](https://docs.docker.com/get-docker/) & [Docker Compose](https://docs.docker.com/compose/) installed on your machine.

### Running with Docker Compose
```bash
# 1. Clone the repository
git clone [https://github.com/your-username/neuralmark.git](https://github.com/your-username/neuralmark.git)
cd neuralmark

# 2. Build and start services
docker-compose up --build

# 3. Access the applications
# Frontend Dashboard -> http://localhost:5173
# FastAPI Interactive Docs -> http://localhost:8000/docs
