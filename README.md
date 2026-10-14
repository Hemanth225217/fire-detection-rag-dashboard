# Cloud RAG Drone Fire Detection: RAG Pipeline and Dashboard

**Hemanth S (23BIT0411)**, BITE412L Cloud Computing, VIT (2025–2026), guided by Priya V

This repository holds my part of the team project *Cloud-Based Retrieval-Augmented Drone Framework for Early Forest Fire Detection and Response Planning*.
The complete integrated system is in the team repository:
**https://github.com/Veladicodes/cloud-rag-drone-fire-detection**

| Team member | Module |
| :--- | :--- |
| Adithya A (23BIT0323) | YOLOv8 edge detection: training and evaluation (`ai/models`, `ai/evaluation`) |
| Pranitha N S (23BIT0320) | FastAPI backend, database, Azure/Terraform infrastructure |
| **Hemanth S (23BIT0411)** | **RAG response-planning pipeline and React dashboard (this repo)** |

## What is in this repo

```
ai/rag/                 RAG pipeline
  ingest.py             chunk SOP text (500 chars, 50 overlap) and build the FAISS index
  embeddings.py         sentence-transformers all-mpnet-base-v2 embeddings
  retrieve.py           top-k FAISS retrieval for an incident
  orchestrate.py        bounded, citation-required prompt -> Gemini 2.5 Flash (or offline mock)
  evaluate_rag.py       BERTScore + hallucinated-source rate evaluation
  reference_plans/      hand-written expert reference plans used by the evaluation
data/knowledge-base/    wildland-firefighting SOP corpus (the only source the LLM may cite)
frontend/               React 18 + Vite + Leaflet dashboard: incident map, alert feed, plan viewer
results/rag-metrics/    measured RAG evaluation output
testing/unit/test_rag_retrieve.py   retrieval-ordering test
docs/research/rag-response.md       RAG design notes
```

## Results (from the Review 2 report)

| Metric | Result | Target |
| :--- | :--- | :--- |
| Hallucinated-source rate | 0.0% | ≤ 1% (met) |
| Mean BERTScore F1 vs. 4 expert plans | 0.827 | none set |
| Inline citation coverage | 25 / 27 claim lines | none set |

Measured on 4 synthetic incidents over a 5-document SOP corpus. That checks the grounding mechanism; it is not a production-scale benchmark.

## Running

These modules plug into the shared backend (`backend.core.config`), so they run as part of the full system.
Use the team repository's quick start (`make setup && make index && make backend`, then `cd frontend && npm install && npm run dev`).
The dashboard reads the API address from `VITE_API_BASE`.
