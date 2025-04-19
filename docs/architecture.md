# Cognisphere Technical Architecture

This document outlines the internal design and behavioral logic of the Cognisphere memory model — a multi-layered, heat-driven, semantically linked cognitive memory system for synthetic reasoning agents and large language models (LLMs).

The Cognisphere transforms static token-limited inputs into a dynamic, persistent, and intelligent memory scaffold, inspired by the layered architecture of the human brain.

---

## 🧱 Core Architectural Layers

Cognisphere divides memory into three semantically stratified layers. Each layer is optimized for different levels of access frequency, data size, and contextual value.

### 1. Surface Index (Immediate Memory)

- **Data Stored**: Summaries, semantic embeddings, timestamps, access counters, metadata tags, and heat scores
- **Storage Type**: Lightweight in-memory key-value DB or fast-access disk store (e.g., SQLite, DuckDB)
- **Function**: Serves as a global index and first-layer filter for all memory queries

Characteristics:
- Always-on memory
- Low token cost to query or load
- Enables rapid semantic filtering
- Stores active references and recently accessed items

### 2. Mid-Layer Memory (Working Memory)

- **Data Stored**: Compressed or summarized semantic text chunks grouped by topic or entity
- **Storage Type**: Document store, compressed vector databases, LLM-friendly summaries
- **Function**: Holds “warm” memory — information relevant to ongoing tasks or recently used material

Characteristics:
- Medium-cost to expand and pass to model
- Efficient for groupings like “project history,” “personas,” “event chains”
- Can be promoted to surface based on usage

### 3. Core Memory (Long-Term Archive)

- **Data Stored**: Archived full documents, deeply compressed references, inactive or aged knowledge
- **Storage Type**: Cold storage or disk-based blob stores (e.g., Parquet, Zstandard, object DBs)
- **Function**: Stores everything not immediately useful, but valuable under deep or rare queries

Characteristics:
- High token cost to use directly
- Accessed only through **core sample retrieval**
- Acts as the long-term memory substrate of the system

---

## 🔥 Heat Engine: Dynamic Memory Prioritization

The **Heat Engine** controls movement between layers by tracking:

- **Recency of Access**: How recently a chunk was referenced
- **Access Frequency**: How often a chunk is used overall
- **Link Activity**: How connected a chunk is to others in recent queries

Heat score = `α × recent_use + β × total_accesses + γ × link_strength` (tunable params)

### Memory Lifecycle:

1. Cold chunks decay toward the core
2. Hot chunks rise toward the surface
3. Spikes in access or linking trigger promotion

Decay functions and scoring models can be linear, exponential, or sigmoid depending on system behavior and data volatility.

---

## 🧬 Core Sample Retrieval Process

This mechanism enables deep memory access without loading entire long-term archives.

### When it's triggered:
- Surface or mid-layer context fails to satisfy a query
- A concept is semantically linked to archived content
- A user explicitly requests deep context

### Retrieval Steps:

1. **Query vectorization** using LLM embedding
2. **Surface scan**: Try semantic match from index
3. **Link resolution**: Traverse neural pathways for related memory chunks
4. **Vertical slice**: Retrieve associated chunks from surface, mid-layer, and core
5. **Expansion**: Temporarily decompress or summarize core content
6. **Model injection**: Pass only the relevant material into LLM context

After interaction:
- Heat scores are updated for touched chunks
- Promotion/demotion decisions are made
- New links are added or strengthened

### Core Sample Benefits:

- Avoids full reload of token-heavy context
- Provides high-relevance, low-cost answers
- Enables rare memory access without latency spike

---

## 🔗 Neural Pathways: Semantic Linking

Each memory chunk maintains soft links to other chunks using semantic similarity, co-access patterns, and manual tagging.

### Link Strength Increases When:

- Two chunks are queried together
- A core sample reveals multiple connected chunks
- A user manually creates a reference or bookmark

### Link Decay Rules:

- Unused links fade over time
- Outdated links can be removed during garbage collection

Link networks form a semantic memory graph that enables non-linear, cross-topic recall — similar to how the brain activates related concepts.

---

## 📊 Heat Score Simulation Example

| Chunk ID | Topic                      | Last Access     | Access Count | Heat Score |
|----------|----------------------------|------------------|--------------|------------|
| c001     | Token parser change log    | 2025-04-18       | 12           | 0.93       |
| c047     | Prompt engineering         | 2025-03-02       | 3            | 0.42       |
| c089     | Obsolete vendor contract   | 2024-12-01       | 0            | 0.03       |

Scores update dynamically with use and link traversal.

---

## 🧪 Memory Chunk Format (Schema)

```json
{
  "chunk_id": "c023",
  "summary": "2021 objections to synthetic cognition",
  "embedding": [[...]],
  "heat_score": 0.92,
  "path": "/core/2021_objections.txt",
  "last_accessed": "2025-04-18",
  "linked_to": ["c024", "c031"],
  "topic": "synthetic cognition"
}
```

Optional fields may include: compression ratio, LLM summary hash, relevance tags, expiration dates.

---

## ⚖️ Comparison with Other Memory Strategies

| Feature                  | RAG | Long Context (GPT-4) | Cognisphere |
|--------------------------|-----|----------------------|-------------|
| Token Window Limits      | ✅  | ✅                   | 🔥 Bypassed |
| Persistent Memory        | ❌  | ❌                   | ✅          |
| Usage-Based Promotion    | ❌  | ❌                   | ✅          |
| Semantic Linking         | ❌  | ❌                   | ✅          |
| Core Sample Retrieval    | ❌  | ❌                   | ✅          |
| Graph Memory Navigation  | ❌  | ❌                   | ✅          |

---

## 📦 Implementation Strategy (Pilot MVP)

| Week | Task                                         |
|------|----------------------------------------------|
| 1    | Chunking + Index Pipeline                    |
| 2    | Embedding Generation + Storage               |
| 3    | Retrieval Layer + Surface Logic              |
| 4    | Heat Map Scoring + Promotion Logic           |
| 5    | Core Memory Access + Neural Pathway Setup    |
| 6    | Pilot Test: Ingest 100 docs, Live Query Test |

---

Cognisphere redefines what memory means in an AI system — not just storage, but *priority-driven, query-reactive, self-organizing cognitive scaffolding*.

