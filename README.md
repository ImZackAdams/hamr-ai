# HAMR  
**Hierarchical Adaptive Memory Retrieval**  
for LLMs that aren’t goldfish 🧠🔥

---

## 🚀 TL;DR  
LLMs are sharp... for like 4K tokens. Then they forget everything.  
Context limits kill long chats. Current RAG? Static, dumb, bloated.  
**HAMR** is layered memory—short, mid, long-term—ranked by relevance, recency, and importance.  
We build the prompt on the fly. No filler. No forgetting. Just results.

---

## 🤮 The Problem  
LLMs can't remember anything past the current prompt. You’ve got two bad options:

- **Keep it all** → bloated prompt, wasted tokens  
- **Trim it down** → lose the plot

RAG helps, kinda...  
But it's a glorified chunk retriever. No memory, no prioritization, no structure. Just "semantic similarity" and vibes.

---

## 🔨 The HAMR Fix

### 🧱 Layered Memory Stores  
1. **Short-Term**  
   - Raw recent chat turns  
   - Just text, no processing  

2. **Mid-Term**  
   - Event-based summaries  
   - Triggered on intent shifts, decisions, milestones  

3. **Long-Term**  
   - Abstracted topics  
   - Knowledge graphs, clusters, core facts  

---

### ⚡ Runtime Query Flow  
Every query triggers:  
- **Embedding** of the new question  
- **Scoring** all memory chunks on:
  - Semantic relevance
  - Recency (decay over time)
  - Importance (custom/manual/learned)
- **Filtering** to fit token budget  
- **Prompt Assembly** in priority order  
  - Recent beats old  
  - Raw beats summarized  
  - Junk gets dropped

---

## 📈 Why This Matters  
LLMs without memory are toys.  
You want real assistants? Real narratives? Real tools?

You need memory.

### 📊 Metrics We Care About  
- **Recall Accuracy**: remembers critical info over time  
- **Token Efficiency**: tokens per useful detail  
- **Coherence**: does it all make sense?  
- **Latency**: is it fast enough to ship?

---

## 🧠 Use Cases  
- Multi-day customer support that doesn’t lose your thread  
- Enterprise tools that know more than a PDF  
- Story agents that evolve like D&D, not goldfish

---

## ⚠️ What Still Sucks  
- Summaries might hallucinate  
- Big topic shifts can confuse the retrieval  
- Summarization/indexing adds compute overhead

But none of that’s fatal. It’s just engineering.

---

## 🛠️ Next Steps

### 0. Prototype It  
- Manual layered store  
- FAISS / Chroma / whatever  
- Hacky prompt builder

### 1. Benchmark  
- Long-context eval sets  
- New evals for memory coherence

### 2. Open Source  
- Clean repo  
- Plug-and-play with open LLMs  
- Memory inspector / override tools

### 3. Stretch Goals  
- Multi-agent memory sync  
- Streaming input support  
- User curation & privacy tools

---

## 🙋‍♂️ Why Bother?  
Because stateless transformers suck at memory.  
This isn’t AGI. This is just **cache + search + relevance scoring**.

LLMs are compressed intelligence.  
**HAMR is compressed memory.**

Let’s build it.

---

> Made by people who hate forgetting stuff.  
> PRs welcome.  
