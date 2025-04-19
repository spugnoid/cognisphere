# Cognisphere Architecture

This document outlines the technical structure and logic of the Cognisphere memory model.

## 🧱 System Layers

### 1. Surface Index
- **Semantic summaries**
- **Embeddings**
- **Metadata (tags, timestamps)**
- **Heat score**

🧠 Always loaded in memory or lightweight DB

---

### 2. Mid-Layer
- **Compressed semantic chunks**
- **Grouped by topic**
- **Expanded on demand**

---

### 3. Core Memory
- **Archived or deeply compressed data**
- **Token-expensive**
- **Accessed only via "core samples" for deep queries**

---

## 🔥 Heat Engine

Tracks recency and frequency of access. Adjusts memory layer position:

- **Hot = Promote to surface**
- **Cold = Demote or compress**
- Strengthens neural links between co-accessed chunks

## 📦 Memory Chunk Schema (JSON)

```json
{
  "chunk_id": "c023",
  "summary": "2021 objections to synthetic cognition",
  "embedding": [[...]],
  "heat_score": 0.92,
  "path": "/core/2021_objections.txt",
  "last_accessed": "2025-04-18",
  "linked_to": ["c024", "c031"]
}
```

## 📊 Heat Score Simulation

| Chunk ID | Topic                      | Last Access     | Access Count | Heat Score |
|----------|----------------------------|------------------|--------------|------------|
| c001     | Token parser change log    | 2025-04-18       | 12           | 0.93       |
| c047     | Prompt engineering         | 2025-03-02       | 3            | 0.42       |
| c089     | Obsolete vendor contract   | 2024-12-01       | 0            | 0.03       |

## ⚖️ Comparison Table

| Feature                  | RAG | Long Context | Cognisphere |
|--------------------------|-----|--------------|-------------|
| Token Limit              | ✅  | ✅           | 🔥 Bypassed |
| Persistent Memory        | ❌  | ❌           | ✅          |
| Usage-Based Recall       | ❌  | ❌           | ✅          |
| Intelligent Linking      | ❌  | ❌           | ✅          |

## 🛠️ Build Timeline (MVP – 6 Weeks)

| Week | Task                                         |
|------|----------------------------------------------|
| 1    | Chunking + Index Pipeline                    |
| 2    | Embedding Generation + Storage               |
| 3    | Retrieval Layer + Surface Logic              |
| 4    | Heat Map Scoring + Promotion Logic           |
| 5    | Core Memory Access + Neural Pathway Setup    |
| 6    | Pilot Test: Ingest 100 docs, Live Query Test |

---

Cognisphere offers a new way to think about memory in AI — modular, intelligent, and aligned with how we actually think.
