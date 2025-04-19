# Cognisphere

Cognisphere is a modular memory architecture for LLMs that emulates human-like memory recall. It selectively loads relevant content from a structured memory sphere—prioritizing hot, recent, or linked information and enabling persistent, scalable context without token bloat.

## 🔍 Overview

Cognisphere introduces a 3-layer system:
- **Surface Index**: Summaries, embeddings, heat scores.
- **Mid-Layer**: Expandable compressed semantic memory.
- **Core**: Deeply compressed or archived knowledge.

A dynamic **Heat Engine** adjusts memory availability based on access patterns.

## 📁 Structure

- `docs/`: Technical PDFs and documentation
- `schemas/`: JSON formats for memory chunks and simulations
- `timeline/`: MVP development schedule

## 🧪 Features

| Feature                  | GPT-4 Context | Cognisphere |
|--------------------------|---------------|-------------|
| Token Limits             | ✅            | 🔥 Bypassed |
| Persistent Memory        | ❌ / Partial  | ✅          |
| Usage-Based Recall       | ❌            | ✅          |
| Core Sampling Efficiency | ❌            | ✅          |

## 🛠️ MVP Timeline

See `timeline/MVP_Build_Timeline.md`.

## 🔐 License

MIT (TBD)
