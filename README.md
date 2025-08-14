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

LLMs crush bite sized tasks, then face plant on long hauls.  
Fixed context is the straight jacket: keep every token and pay through the nose, or prune history and lobotomize the model.  
RAG patches the hole, but vanilla vector search is flat, frozen, and time blind.

**HAMR** bolts on a human style memory stack:

* **Short Term** — raw turns, hot off the wire  
* **Mid Term** — summaries when something actually happens  
* **Long Term** — distilled truth, graphs, core facts  

Every query runs the gauntlet: score chunks on meaning, freshness, and weight, then spit out the leanest prompt per token.  
Your assistant remembers what matters, forgets the noise, and never blows the context budget.

---

## Motivation

* Real assistants live for weeks, not seconds.  
* Token ceilings are still physics; one-hundred-k contexts just move the pain to your wallet.  
* We need first class memory, not another prompt engineering hack.

---

## Problem Statement

1. **Context Explosion** — chat history balloons way past the window.  
2. **Static Retrieval** — cosine similarity is not memory; it is blind to time and priority.  
3. **Lossy Truncation** — naïve trimming nukes tomorrow’s critical detail.  

---

## Related Work

### Conversation Buffer  
**✔️** Dead simple, keeps everything  
**❌** Alzheimer’s for anything beyond the cap

### Summary Memory (LangChain, Zep)  
**✔️** Cheap on tokens  
**❌** Summaries hallucinate; fine grained recall is gone

### Vector Retrieval  
**✔️** Pulls arbitrary past chunks  
**❌** Ignores recency and hierarchy

### Long Context Transformers (Claude one-hundred-k, Gemini)  
**✔️** End to end attention  
**❌** Expensive, slower, still filter hungry

### Knowledge Graph Memory (GraphRAG, Graphiti)  
**✔️** Structured facts you can query  
**❌** Heavy ingest, brittle updates  

**HAMR cherry picks the wins and ditches the baggage.**

---

## HAMR Architecture

### Memory Flow Pipeline

1. **User Query**  
2. → embed and tag metadata  
3. → hit **Memory Store**  
   * **Short Term**: raw turns (TTL ≈ fifty)  
   * **Mid Term**: summaries (TTL in days)  
   * **Long Term**: forever facts (no TTL)  
4. → score chunks: similarity, recency, importance  
5. → rank and trim to fit the token cap  
6. → **Prompt Assembly Engine**: system + memory + query  
7. → model answers, rinse, repeat  

---

## Memory Layers

### Short Term Layer
* Raw JSON or plain text turns  
* Evict by age or buffer size

### Mid Term Layer
* Summaries on intent flips, decisions, or timeouts  
* Extract then abstract to curb hallucinations

### Long Term Layer
* Graph nodes, user profiles, immutable facts  
* Store in Neo4j, SQLite, or plain JSON  
* Importance: pin it or learn it

---

## Runtime Retrieval Algorithm

1. **Embed** — create vector for the new turn.  
2. **Score** — `score(c) = α·sim + β·recency + γ·importance` (tune α β γ, change the game).  
3. **Rank and Filter** — keep the top until you are under budget.  
4. **Assemble** — fresh raw > summary > lore.  
5. **Generate** — fire the prompt at the model.  
6. **Update Memory** — log the turn, roll summaries, cement facts.

---

## Evaluation Methodology

| Metric            | Description                                   |
|-------------------|-----------------------------------------------|
| Recall Accuracy   | answers that hinge on past context            |
| Token Efficiency  | tokens burned per useful detail               |
| Coherence Score   | narrative sanity over time                    |
| Latency           | wall clock from query to response             |

### Benchmark Suites

* **Long Term Chat** — one-hundred-fifty turn synthetic support runs  
* **Narrative QA** — clip back questions on early plot hooks  
* **Enterprise FAQ** — multi session customer drills  

---

## Implementation Roadmap

| Phase | Deliverable                                          | Timeline |
|-------|------------------------------------------------------|----------|
| Zero  | skeleton demo: FAISS + SQLite layers                 | week one |
| One   | bench harness: Jupyter eval scripts                  | week three |
| Two   | OSS drop: repo + REST API                            | month two |
| Three | integrations: LangChain, Slack bot, Zep plugin       | month three |
| Four  | extras: streaming ingest, multi agent gossip         | month six |

---

## Risks and Mitigations

| Risk                  | Impact                     | Mitigation                                    |
|-----------------------|----------------------------|-----------------------------------------------|
| Summary hallucination | poisoned long term memory  | extractive first chain, guardrail checks      |
| Retrieval latency     | sluggish UX                | embedding cache, ANN search, async summaries  |
| Importance bias       | pinned data dominates      | re weight sweeps, user feedback loops         |

---

## Call to Action

HAMR is an invite to build LLMs with a working brain.
