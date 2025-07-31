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

Large language models excel at short tasks yet struggle to retain long-term context.  
Fixed context limits force developers into a dilemma: keep every past turn and inflate token usage, or drop history and risk incoherent responses.  
Retrieval-Augmented Generation helps, but ordinary vector search is flat, static, and ignores temporal importance.

**HAMR** introduces a layered memory structure that mimics human recall:

- **Short Term** – recent raw turns  
- **Mid Term** – event-driven summaries  
- **Long Term** – abstract knowledge graphs or fact clusters  

For each user query, HAMR scores every memory chunk by semantic relevance, recency decay, and user-defined importance, then assembles a lean prompt that maximizes useful information per token.  
The result is an assistant that remembers what matters, forgets what does not, and stays within context constraints.

---

## Motivation

- Modern assistants must sustain multi-day conversations, iterative workflows, and evolving knowledge bases.  
- Token limits remain a hard boundary. Even with 100 K-token contexts, cost and latency grow rapidly.  
- A principled memory layer is essential for practical long-horizon artificial intelligence.

---

## Problem Statement

1. **Context Explosion** – Long chats accumulate far more tokens than a single prompt can hold.  
2. **Static Retrieval** – Plain vector search ranks only on similarity and ignores time or importance.  
3. **Lossy Truncation** – Simple pruning discards valuable details with no regard for future relevance.  

---

## Related Work

### Conversation Buffer  
**✔️** Easy to implement, preserves detail  
**❌** Forgets older key facts

### Summary Memory (LangChain, Zep)  
**✔️** Good token efficiency  
**❌** Hallucination risk, lacks fine-grained recall

### Vector Retrieval  
**✔️** Can fetch arbitrary history  
**❌** No notion of recency or priority

### Long Context Transformers (e.g., Claude 100 K, Gemini)  
**✔️** Full-text attention  
**❌** High cost, slower, still needs filtering

### Knowledge Graph Memory (GraphRAG, Graphiti)  
**✔️** Structured and queryable facts  
**❌** Complex ingestion, slow updates  

**HAMR combines the strengths of all the above approaches while mitigating their limitations.**

---

## HAMR Architecture

### Memory Flow Pipeline

1. **User Query**  
2. → Embed + extract metadata  
3. → Search in **Memory Store**  
   - **Short Term**: raw turns (TTL ≈ 50)  
   - **Mid Term**: summaries (TTL in days)  
   - **Long Term**: persistent facts (no TTL)  
4. → Score each memory chunk: semantic similarity, recency, importance  
5. → Rank and trim to fit the token budget  
6. → **Prompt Assembly Engine** combines system prompt + memory + query  
7. → Language model generates response  

---

## Memory Layers

### Short Term Layer
- Raw JSON or plain-text message objects  
- Eviction policy: time-based decay or size cap

### Mid Term Layer
- Summaries triggered by intent shifts, decision points, or elapsed turns  
- Generated via extract-then-abstract prompting to reduce hallucination

### Long Term Layer
- Knowledge-graph nodes, user profiles, persistent facts  
- Storage: Neo4j, SQLite, or JSON document store  
- Importance scoring: manual (pinned) or learned (reinforced when referenced)

---

## Runtime Retrieval Algorithm

1. **Embed** – Compute embeddings for the incoming user turn.  
2. **Score** – For every memory chunk `c`, compute score(c) = α · similarity(c, query) + β · recency(c) + γ · importance(c) where `α`, `β`, and `γ` are tunable weights.
3. **Rank and Filter** – Sort by score and trim until the prompt fits the budget.  
4. **Assemble** – Add (i) recent raw turns, (ii) mid-term summaries, (iii) top long-term facts.  
5. **Generate** – Send the prompt to the language model.  
6. **Update Memory** – Store the new turn, create/extend summaries, and persist confirmed facts.

---

## Evaluation Methodology

| Metric            | Description                                   |
|-------------------|-----------------------------------------------|
| Recall Accuracy   | Correct answers that rely on past context     |
| Token Efficiency  | Prompt tokens per salient fact                |
| Coherence Score   | Logical consistency across turns              |
| Latency           | End-to-end time per request (incl. retrieval) |

### Benchmark Suites

- **Long-Term Chat** – 150-turn synthetic support conversations  
- **Narrative QA** – Story agents queried about early plot points  
- **Enterprise FAQ** – Multi-session customer question sequences  

---

## Implementation Roadmap

| Phase | Deliverable                                          | Timeline |
|-------|------------------------------------------------------|----------|
| 0     | Prototype: FAISS + SQLite memory demo                | Week 1   |
| 1     | Benchmark: Jupyter evaluation harness                | Week 3   |
| 2     | Reference implementation: open-source repo & REST API| Month 2  |
| 3     | Ecosystem: LangChain connector, Slack bot, Zep plugin| Month 3  |
| 4     | Stretch goals: streaming input, multi-agent sync     | Month 6  |

---

## Risks and Mitigations

| Risk                  | Impact                     | Mitigation                                          |
|-----------------------|----------------------------|-----------------------------------------------------|
| Summary hallucination | Incorrect long-term memory | Extractive-abstractive chains, validation rules     |
| Retrieval latency     | Poor user experience       | Embedding cache, approximate search, async summary  |
| Importance bias       | Pinned data dominates      | Periodic re-weighting, user feedback                |

---

## Call to Action

HAMR invites the community to build smarter, long-lived AI systems.

- **🛠  Try the prototype**  
- **🐛  Open issues** – suggest scoring tweaks or connectors  
- **💬  Join the conversation** on pushing LLMs past goldfish memory  

> **Give language models a brain that remembers.**

