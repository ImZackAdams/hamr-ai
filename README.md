# HAMR  
**Hierarchical Adaptive Memory Retrieval for Large Language Models**  
_White Paper • GitHub README Format_

---

## Table of Contents
1. [Abstract](#abstract)
2. [Motivation](#motivation)
3. [Problem Statement](#problem-statement)
4. [Related Work](#related-work)
5. [HAMR Architecture](#hamr-architecture)
6. [Memory Layers](#memory-layers)
7. [Runtime Retrieval Algorithm](#runtime-retrieval-algorithm)
8. [Evaluation Methodology](#evaluation-methodology)
9. [Implementation Roadmap](#implementation-roadmap)
10. [Risks and Mitigations](#risks-and-mitigations)
11. [Call to Action](#call-to-action)

---

## Abstract

LLMs crush bite-sized tasks, then face-plant on long hauls.  
Fixed context is the straight-jacket: keep every token and pay through the nose, or prune history and lobotomize the model.  
RAG patches the hole, but vanilla vector search is flat, frozen, and time-blind.

**HAMR** bolts on a human-style memory stack:

- **Short Term** – raw turns, hot off the wire  
- **Mid Term** – summaries when something actually happens  
- **Long Term** – distilled truth, graphs, core facts  

Every query runs the gauntlet: score chunks on meaning, freshness, and weight, then spit out the leanest prompt per token.  
Your assistant remembers what matters, forgets the noise, and never blows the context budget.

---

## Motivation

- Real assistants live for weeks, not seconds.  
- Token ceilings are still physics; 100 K contexts just move the pain to your wallet.  
- We need first-class memory, not another prompt engineering hack.

---

## Problem Statement

1. **Context Explosion** – Chat history balloons way past the window.  
2. **Static Retrieval** – Cosine similarity ≠ memory; it’s blind to time and priority.  
3. **Lossy Truncation** – Naïve trimming nukes tomorrow’s critical detail.  

---

## Related Work

### Conversation Buffer  
**✔️** Dead simple, keeps everything  
**❌** Alzheimer’s for anything beyond the cap

### Summary Memory (LangChain, Zep)  
**✔️** Cheap on tokens  
**❌** Summaries hallucinate; fine-grained recall is gone

### Vector Retrieval  
**✔️** Pulls arbitrary past chunks  
**❌** Ignores recency and hierarchy

### Long Context Transformers (Claude 100 K, Gemini)  
**✔️** End-to-end attention  
**❌** Expensive, slower, still filter-hungry

### Knowledge Graph Memory (GraphRAG, Graphiti)  
**✔️** Structured facts you can query  
**❌** Heavy ingest, brittle updates  

**HAMR cherry-picks the wins and ditches the baggage.**

---

## HAMR Architecture

### Memory Flow Pipeline

1. **User Query**  
2. → Embed & tag metadata  
3. → Hit **Memory Store**  
   - **Short Term**: raw turns (TTL ≈ 50)  
   - **Mid Term**: summaries (TTL in days)  
   - **Long Term**: forever facts (no TTL)  
4. → Score chunks: similarity, recency, importance  
5. → Rank + trim to fit the token cap  
6. → **Prompt Assembly Engine**: system + memory + query  
7. → Model answers, rinse, repeat  

---

## Memory Layers

### Short Term Layer
- Raw JSON/plain text turns  
- Evict by age or buffer size

### Mid Term Layer
- Summaries on intent flips, decisions, or timeouts  
- Extract-then-abstract to curb hallucinations

### Long Term Layer
- Graph nodes, user profiles, immutable facts  
- Store in Neo4j, SQLite, or plain JSON  
- Importance: pin it or learn it

---

## Runtime Retrieval Algorithm

1. **Embed** – Create vector for the new turn.  
2. **Score** – `score(c) = α·sim + β·recency + γ·importance` — tune α/β/γ, change the game.  
3. **Rank / Filter** – Keep the top until you’re under budget.  
4. **Assemble** – Fresh raw > summary > lore.  
5. **Generate** – Fire the prompt at the model.  
6. **Update Memory** – Log the turn, roll summaries, cement facts.

---

## Evaluation Methodology

| Metric            | Description                                   |
|-------------------|-----------------------------------------------|
| Recall Accuracy   | Answers that hinge on past context            |
| Token Efficiency  | Tokens burned per useful detail               |
| Coherence Score   | Narrative sanity over time                    |
| Latency           | Wall-clock from query to response             |

### Benchmark Suites

- **Long-Term Chat** – 150-turn synthetic support runs  
- **Narrative QA** – Clip-back questions on early plot hooks  
- **Enterprise FAQ** – Multi-session customer drills  

---

## Implementation Roadmap

| Phase | Deliverable                                          | Timeline |
|-------|------------------------------------------------------|----------|
| 0     | Skeleton demo: FAISS + SQLite layers                 | Week 1   |
| 1     | Bench harness: Jupyter eval scripts                  | Week 3   |
| 2     | OSS drop: repo + REST API                            | Month 2  |
| 3     | Integrations: LangChain, Slack bot, Zep plugin       | Month 3  |
| 4     | Extras: streaming ingest, multi-agent gossip         | Month 6  |

---

## Risks and Mitigations

| Risk                  | Impact                     | Mitigation                                    |
|-----------------------|----------------------------|-----------------------------------------------|
| Summary hallucination | Poisoned long-term memory  | Extractive-first chain, guardrail checks      |
| Retrieval latency     | Sluggish UX                | Embedding cache, ANN search, async summaries  |
| Importance bias       | Pinned data dominates      | Re-weight sweeps, user feedback loops         |

---

## Call to Action

HAMR is an invite to build LLMs with a working brain.

- **🛠  Kick the tires** – run the demo  
- **🐛  File issues** – tweak scoring, add stores  
- **💬  Join us** – let’s kill goldfish memory together  

> **Give language models a real memory.**
