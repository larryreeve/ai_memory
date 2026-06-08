# AI Memory Usage — Research Starter Brief

_Last updated: June 8, 2026_

## 1. Purpose

This document is a starting point for researching how AI systems use memory: how memory is represented, stored, retrieved, updated, evaluated, and productized in modern LLM assistants and agent systems.

The focus is on practical AI memory systems rather than biological memory theory, although some systems borrow concepts such as episodic memory, semantic memory, procedural memory, forgetting, consolidation, reflection, and temporal organization.

---

## 2. Core Concepts

### 2.1 Why memory matters

LLMs are naturally stateless unless conversation history, retrieved documents, user profile data, tool results, or external memory stores are supplied at inference time. Memory systems are used to:

- Maintain continuity across conversations.
- Personalize responses based on user preferences and history.
- Preserve project context across sessions.
- Support long-running agents that learn from prior actions.
- Reduce repeated user explanation.
- Improve task performance when relevant historical context is needed.
- Manage context-window limits by storing, summarizing, and selectively retrieving prior information.

### 2.2 Common memory types

| Memory type | Description | Examples |
|---|---|---|
| Short-term / working memory | Context used during the current task or conversation. | Current chat messages, scratchpad, active tool outputs. |
| Episodic memory | Records of past events or interactions. | “On March 3, the user said they prefer X.” |
| Semantic memory | Stable facts, preferences, or knowledge extracted from interactions. | User preferences, project facts, durable profile details. |
| Procedural memory | Instructions, habits, workflows, or learned ways of doing things. | Coding style, review checklist, preferred writing structure. |
| Reflective / consolidated memory | Summaries or abstractions created from many episodes. | “The user tends to prefer practical, concise advice.” |
| External knowledge memory | Retrieved facts from files, databases, vector stores, graphs, or APIs. | RAG documents, business records, CRM data. |
| Temporal memory | Memory with explicit time, versioning, invalidation, or change tracking. | “User used to prefer Adidas, but later switched to Nike.” |

---

## 3. Research Papers and Surveys

### 3.1 Surveys

#### A Survey on the Memory Mechanism of Large Language Model Based Agents
- Link: https://arxiv.org/abs/2404.13501
- Why it matters: Broad survey covering what memory is, why it is needed, memory module designs, evaluation approaches, applications, limitations, and future directions.
- Use this as the best high-level entry point.

#### From Storage to Experience: A Survey on the Evolution of LLM Agent Memory Mechanisms
- Link: https://www.preprints.org/manuscript/202601.0618
- Why it matters: Frames memory evolution as moving from storage, to reflection, to experience abstraction. Useful for understanding where the field may be heading.

### 3.2 Foundational and influential systems

#### MemGPT: Towards LLMs as Operating Systems
- Link: https://arxiv.org/abs/2310.08560
- Project: https://research.memgpt.ai/
- Why it matters: Introduces “virtual context management,” inspired by operating-system memory hierarchies. MemGPT manages memory tiers and moves information in and out of the active context window.
- Key idea: Treat the LLM context window like limited RAM and external memory like disk or virtual memory.

#### LongMem: Augmenting Language Models with Long-Term Memory
- Link: https://arxiv.org/abs/2306.07174
- GitHub: https://github.com/Victorwz/LongMem
- Why it matters: Proposes augmenting language models with a long-term memory mechanism that can cache and retrieve long histories.
- Key idea: Separate the model backbone from memory encoding/retrieval mechanisms.

#### MemoryBank: Enhancing Large Language Models with Long-Term Memory
- Link: https://arxiv.org/abs/2305.10250
- AAAI version: https://ojs.aaai.org/index.php/AAAI/article/view/29946
- GitHub: https://github.com/zhongwanjun/MemoryBank-SiliconFriend
- Why it matters: Uses a memory mechanism for long-term companion-style interaction, including memory updating and forgetting inspired by the Ebbinghaus forgetting curve.
- Key idea: Memory should not only store facts; it should evolve, reinforce, and forget.

#### Generative Agents: Interactive Simulacra of Human Behavior
- Link: https://arxiv.org/abs/2304.03442
- Why it matters: Popularized memory streams, reflection, and planning for believable long-running agents.
- Key idea: Agents can retrieve relevant memories, reflect on them, and use them to plan future actions.

#### A-Mem: Agentic Memory for LLM Agents
- Link: https://openreview.net/forum?id=FiM0M8gcct
- Why it matters: Explores more adaptive memory organization for agents, moving beyond simple fixed storage/retrieval patterns.

#### Zep: A Temporal Knowledge Graph Architecture for Agent Memory
- Link: https://arxiv.org/abs/2501.13956
- Why it matters: Presents a temporal knowledge graph approach for enterprise agent memory and compares against existing memory/retrieval benchmarks.
- Key idea: Real-world memory needs time, change tracking, relationship modeling, and structured context assembly.

---

## 4. Evaluation Methods for AI Memory Systems

Memory evaluation is still immature. The central challenge is separating three different capabilities:

1. Did the system store the right memory?
2. Did it retrieve the right memory?
3. Did the model correctly reason over that memory?

A system can fail at any of these layers.

### 4.1 Major benchmark families

#### LongMemEval
- Paper: https://arxiv.org/abs/2410.10813
- Project: https://xiaowu0162.github.io/long-mem-eval/
- GitHub: https://github.com/xiaowu0162/longmemeval
- What it evaluates: Long-term memory in chat assistants.
- Core abilities tested:
  - Information extraction.
  - Multi-session reasoning.
  - Temporal reasoning.
  - Knowledge updates.
  - Abstention when memory is insufficient.
- Why it matters: One of the most directly relevant benchmarks for assistant memory.

#### LoCoMo: Long Conversation Memory
- Paper: https://arxiv.org/abs/2402.17753
- Project: https://snap-research.github.io/locomo/
- GitHub: https://github.com/snap-research/locomo
- What it evaluates: Very long-term conversational memory across multi-session conversations.
- Tasks:
  - Question answering.
  - Event summarization.
  - Multi-modal dialogue generation.
- Why it matters: Tests whether agents can remember and reason over long-running relationships and events.

#### Deep Memory Retrieval / DMR
- Often used by systems such as MemGPT and Zep for retrieval-focused evaluation.
- What it evaluates: Whether the system retrieves the correct memory from a large memory store.
- Limitation: Strong retrieval does not guarantee the final answer is correct.

#### BEAM and newer long-memory stress tests
- Used by some vendor evaluations such as Mem0.
- What it evaluates: Larger-scale retrieval and memory performance under bigger memory stores.
- Caveat: Vendor benchmarks should be read carefully because methodology, datasets, and scoring may differ.

### 4.2 Evaluation dimensions

| Dimension | What to test | Example question |
|---|---|---|
| Recall accuracy | Can the system retrieve the right fact? | “What hotel did I say I liked last month?” |
| Relevance | Does it avoid retrieving irrelevant memory? | User asks about Java; system should not inject vacation preferences. |
| Temporal reasoning | Can it distinguish old facts from new facts? | “Do I still prefer Adidas?” after user later says they switched to Nike. |
| Update handling | Can memories be corrected or superseded? | User changes dietary preference or project direction. |
| Conflict resolution | Can it handle contradictory memories? | “I prefer short answers” vs later “give me detailed research briefs.” |
| Abstention | Does it admit when memory is missing? | “What did I say my son’s school was?” when never provided. |
| Privacy and sensitivity | Does it avoid storing inappropriate or sensitive facts? | Health, politics, religion, precise location, financial identifiers. |
| Personalization lift | Does memory improve user-specific usefulness? | Better recommendations based on stable preferences. |
| Robustness | Does memory degrade gracefully when stale/noisy? | Old project notes should not override current instructions. |
| Latency | How fast can memory be retrieved? | Retrieval time under realistic production loads. |
| Token efficiency | How much context is injected? | Does memory reduce or increase prompt bloat? |
| Cost | Cost per memory write/read/reasoning turn. | Vector search + summarization + LLM judge cost. |
| Interpretability | Can users inspect and edit memory? | Memory dashboard, memory file, or explicit memory API. |
| Governance | Can memory be scoped and controlled? | Personal vs work memory; project-only memory; tenant controls. |

### 4.3 Practical evaluation design

A practical evaluation suite should include at least five layers:

#### Layer 1 — Memory write tests
Check whether the system stores the right information.

Example:
- User says: “For future retirement discussions, use my income plan, not generic advice.”
- Expected memory: A durable preference about financial-response style.
- Should not store: Unnecessary transient details from the same conversation.

#### Layer 2 — Memory retrieval tests
Check whether relevant memories are retrieved for the right task.

Example:
- User asks: “Should I add a Treasury bond?”
- Expected retrieval: Retirement income plan, guaranteed income goals, existing Treasury allocation.
- Bad retrieval: Unrelated travel or home automation memories.

#### Layer 3 — Reasoning-over-memory tests
Check whether the model uses retrieved memory correctly.

Example:
- If the memory says the user wants guaranteed income to cover fixed expenses, the answer should distinguish guaranteed income from discretionary income.

#### Layer 4 — Update and forgetting tests
Check whether new information supersedes old information.

Example:
- Session 1: “I prefer active vacations.”
- Session 2: “I now prefer quiet, low-crowd trips.”
- Future answer should use the later preference or mention the change.

#### Layer 5 — Safety and governance tests
Check whether memory respects controls and avoids harmful persistence.

Example:
- User says: “Forget that.”
- Expected behavior: Memory should be deleted or no longer used.
- User uses temporary/incognito chat.
- Expected behavior: No memory write.

### 4.4 Useful metrics

| Metric | Meaning |
|---|---|
| Exact match / F1 | Useful for factual memory QA. |
| ROUGE / summary similarity | Useful for event summaries. |
| LLM-as-judge score | Useful but should be audited because judges can be too lenient. |
| Retrieval precision@k | Whether top-k retrieved memories are relevant. |
| Retrieval recall@k | Whether all needed memories appear in retrieved set. |
| MRR / NDCG | Ranking quality of retrieved memories. |
| Contradiction rate | How often memory produces inconsistent answers. |
| Staleness error rate | How often old memory overrides newer facts. |
| Abstention accuracy | Whether the model refuses to invent missing memory. |
| Token cost per answer | How much memory context is injected. |
| Latency p50 / p95 / p99 | Production-readiness of memory retrieval. |
| User correction rate | How often users correct memory-driven responses. |
| Memory bloat rate | Growth of stored memories over time. |
| Sensitive-memory capture rate | How often the system stores things it should not. |

### 4.5 Evaluation warnings

- A long context window is not the same as memory. If a benchmark fits entirely into context, it may test context handling more than true memory.
- Retrieval accuracy is not enough. The final model may still reason incorrectly.
- LLM judges can be unreliable. Use human review for high-stakes memory behavior.
- Vendor benchmark claims should be read as directional unless methodology and datasets are public.
- Memory systems can over-personalize, reinforce outdated assumptions, or inject irrelevant context.
- The hardest real-world cases involve time, contradictions, forgotten preferences, changing goals, and privacy boundaries.

---

## 5. Open Source Projects and Frameworks

### 5.1 Letta / MemGPT
- Website: https://www.letta.com/
- Research: https://research.memgpt.ai/
- GitHub: https://github.com/letta-ai/letta
- Category: Agent memory framework inspired by MemGPT.
- Notes: Good for understanding explicit memory tiers, context management, and agent-controlled memory.

### 5.2 Mem0
- Website: https://mem0.ai/
- GitHub: https://github.com/mem0ai/mem0
- Research: https://mem0.ai/research
- Category: Universal memory layer for agents and apps.
- Notes: Popular developer-oriented memory layer focused on personalization, reduced redundant context, and token-efficient retrieval.

### 5.3 Zep / Graphiti
- Website: https://www.getzep.com/
- Docs: https://help.getzep.com/concepts
- GitHub: https://github.com/getzep/zep
- Paper: https://arxiv.org/abs/2501.13956
- Category: Temporal graph memory for agents.
- Notes: Strong enterprise focus; useful for thinking about memory as a time-aware context graph rather than simple vector search.

### 5.4 LangGraph / LangChain memory
- Memory overview: https://docs.langchain.com/oss/python/concepts/memory
- Long-term memory docs: https://docs.langchain.com/oss/python/langchain/long-term-memory
- Category: Agent framework with short-term and long-term memory patterns.
- Notes: Practical for building memory into tool-using agents. Supports thread-scoped memory and cross-thread stores.

### 5.5 LlamaIndex
- Website: https://www.llamaindex.ai/
- GitHub: https://github.com/run-llama/llama_index
- Category: Data and retrieval framework.
- Notes: More RAG/data orchestration than personal memory, but relevant because many memory systems are built from retrieval pipelines.

### 5.6 MemoryBank / SiliconFriend
- GitHub: https://github.com/zhongwanjun/MemoryBank-SiliconFriend
- Category: Research implementation for long-term companion memory.
- Notes: Useful for studying forgetting/reinforcement and persona adaptation.

### 5.7 ReMe
- GitHub: https://github.com/agentscope-ai/ReMe
- Category: Memory management kit for AI agents.
- Notes: Provides file-based and vector-based memory patterns.

### 5.8 Cline Memory Bank
- Docs: https://docs.cline.bot/best-practices/memory-bank
- Category: File-based coding-agent memory.
- Notes: Uses structured Markdown files to maintain project continuity. Useful because it is explicit, inspectable, and easy to version.

### 5.9 Awesome Memory for Agents lists
- GitHub: https://github.com/TsinghuaC3I/Awesome-Memory-for-Agents
- GitHub: https://github.com/Shichun-Liu/Agent-Memory-Paper-List
- Category: Curated paper/project lists.
- Notes: Good for ongoing tracking of new work.

---

## 6. Vendor and Product Implementations

### 6.1 OpenAI ChatGPT Memory and Dreaming
- Memory FAQ: https://help.openai.com/en/articles/8590148-memory-faq
- Reference saved memories: https://help.openai.com/en/articles/11146739-how-does-reference-saved-memories-work
- Dreaming article: https://openai.com/index/chatgpt-memory-dreaming/
- Category: Consumer and business assistant memory.
- Key ideas:
  - Saved memories.
  - Reference chat history.
  - User controls for memory.
  - Newer “Dreaming” architecture to refresh and organize relevant preferences across conversations.
- Research questions:
  - How does memory consolidation work?
  - How are stale memories detected?
  - How are sensitive memories filtered?
  - How much is explicit memory vs implicit profile inference?

### 6.2 Google Gemini / Vertex AI Memory Bank
- Vertex AI Memory Bank blog: https://cloud.google.com/blog/products/ai-machine-learning/vertex-ai-memory-bank-in-public-preview
- Gemini Enterprise Memory Bank docs: https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/memory-bank
- Category: Managed memory for enterprise agents.
- Key ideas:
  - Dynamically generate long-term memories from user-agent conversations.
  - Personalization across sessions.
  - Managed service approach for agent builders.

### 6.3 Anthropic Claude memory
- Claude API memory tool docs: https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool
- Claude Code memory docs: https://code.claude.com/docs/en/memory
- Category: File/directory-based memory and project memory.
- Key ideas:
  - Memory as a persistent file directory.
  - Claude can create, read, update, and delete memory files.
  - Project-scoped memory for coding workflows.

### 6.4 Microsoft Copilot Memory
- Microsoft 365 Copilot announcement: https://techcommunity.microsoft.com/blog/microsoft365copilotblog/introducing-copilot-memory-a-more-productive-and-personalized-ai-for-the-way-you/4432059
- Microsoft support: https://support.microsoft.com/en-us/microsoft-365-copilot/personalize-what-microsoft-365-copilot-remembers
- GitHub Copilot Memory: https://docs.github.com/en/copilot/how-tos/use-copilot-agents/copilot-memory
- Category: Productivity and coding assistant memory.
- Key ideas:
  - Remember work preferences, recurring topics, and user-specific facts.
  - Organization/enterprise controls.
  - Coding conventions and preferences for GitHub Copilot agents.

### 6.5 Amazon Bedrock AgentCore Memory
- Docs: https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory.html
- Getting started: https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory-get-started.html
- Starter toolkit: https://github.com/aws/bedrock-agentcore-starter-toolkit/blob/main/documentation/docs/user-guide/memory/quickstart.md
- Category: Managed memory infrastructure for enterprise agents.
- Key ideas:
  - Short-term context and long-term knowledge retention.
  - Managed memory resources for agents.
  - Designed to avoid building memory infrastructure from scratch.

### 6.6 Zep
- Website: https://www.getzep.com/
- Docs: https://help.getzep.com/concepts
- Category: Enterprise agent memory and context graph.
- Key ideas:
  - Temporal context graph.
  - Multi-source ingest from chat, documents, business data, and JSON.
  - Context assembly under token budgets.
  - Governance and enterprise retrieval latency claims.

### 6.7 Mem0
- Website: https://mem0.ai/
- GitHub: https://github.com/mem0ai/mem0
- Research: https://mem0.ai/research
- Category: Developer memory layer for agents.
- Key ideas:
  - Persistent user memory.
  - Personalized agent interactions.
  - Token-efficient memory retrieval.
  - Benchmarking against LongMemEval, LoCoMo, and BEAM.

---

## 7. Architecture Patterns

### 7.1 Append-only conversation logs
- Store every turn and retrieve relevant chunks later.
- Simple but can become noisy, expensive, and stale.

### 7.2 Summarized memory
- Periodically summarize conversations into compact facts or summaries.
- Useful for reducing tokens.
- Risk: summaries can drop nuance or encode errors.

### 7.3 Vector-store memory
- Embed memories and retrieve by semantic similarity.
- Common, easy to implement.
- Risk: poor temporal reasoning, weak conflict handling, irrelevant recalls.

### 7.4 Graph memory
- Represent users, entities, facts, events, and relationships as a graph.
- Better for relationships, provenance, and temporal changes.
- More complex to build and govern.

### 7.5 File-based memory
- Store memory as Markdown, JSON, or project files.
- Highly inspectable and versionable.
- Common in coding agents.
- Risk: can become bloated and manually curated.

### 7.6 Hierarchical memory
- Separate working context, recent episodic memory, summaries, durable facts, and archive.
- Similar to operating-system or human-inspired memory hierarchies.
- Useful for long-running agents.

### 7.7 Reflective / consolidation loops
- A background or periodic process reviews interactions and updates memory.
- Similar to “dreaming,” reflection, or sleep-like consolidation.
- Key challenge: deciding what to retain, revise, forget, or promote.

### 7.8 Memory with explicit governance
- Scope memories by user, project, organization, sensitivity, and lifespan.
- Necessary for enterprise use.
- Important controls: view, edit, delete, export, disable, temporary mode, project-only memory.

---

## 8. Practical Research Questions

Use these questions to guide deeper research:

1. What should be stored as memory versus kept only in conversation history?
2. Who decides what becomes memory: the user, the model, rules, or a background consolidation process?
3. How should memory handle time and changing preferences?
4. How should memory distinguish stable user preferences from temporary context?
5. How should sensitive information be filtered or protected?
6. How should a system explain which memory influenced an answer?
7. How can users inspect, correct, and delete memory?
8. How should memory be scoped: global, project, workspace, tenant, or task-specific?
9. How do we evaluate memory separately from retrieval and reasoning?
10. How do we prevent memory bloat and irrelevant personalization?
11. When is simple file-based memory better than vector or graph memory?
12. When does long-context reasoning reduce the need for external memory?
13. How should enterprise memory integrate with source-of-truth systems like CRM, ticketing, docs, and code repositories?
14. What are the risks of memory reinforcing user anxiety, false beliefs, or outdated assumptions?
15. What memory architecture is best for software engineering agents?

---

## 9. Suggested Learning Path

### Step 1 — Start with the survey
Read “A Survey on the Memory Mechanism of Large Language Model Based Agents.” Build a map of terms: short-term, long-term, episodic, semantic, procedural, reflection, retrieval, forgetting, and update.

### Step 2 — Study three architecture patterns
Read MemGPT, MemoryBank, and Zep. These give three distinct mental models:

- MemGPT: memory as virtual context management.
- MemoryBank: memory as evolving user/persona memory with forgetting.
- Zep: memory as temporal enterprise knowledge graph.

### Step 3 — Study evaluation
Read LongMemEval and LoCoMo. Focus on what they test and what they miss.

### Step 4 — Build a toy memory system
Implement a small assistant with:

- A memory write step.
- A memory store.
- A retrieval step.
- A conflict/update policy.
- A memory inspection view.
- A small test suite.

### Step 5 — Compare three implementations
Prototype the same use case with:

- File-based memory.
- Vector-store memory.
- Graph or structured JSON memory.

Compare accuracy, latency, explainability, and maintainability.

---

## 10. Starter Prototype Idea

Build a small “personal research assistant memory” app.

### Features

- Stores durable preferences only when explicitly useful.
- Extracts candidate memories from conversations.
- Assigns each candidate a type: semantic, episodic, procedural, preference, project fact.
- Adds timestamps and source references.
- Retrieves memories for new questions.
- Shows which memories were used.
- Allows edit/delete.
- Runs evaluation scenarios for recall, update, temporal reasoning, and abstention.

### Minimal memory schema

```json
{
  "memory_id": "uuid",
  "user_id": "user_123",
  "scope": "global | project | task",
  "type": "preference | fact | project_context | procedure | episode",
  "content": "User prefers practical, concise financial advice grounded in their retirement income plan.",
  "source": "conversation_id_or_document_id",
  "created_at": "2026-06-08T00:00:00Z",
  "updated_at": "2026-06-08T00:00:00Z",
  "valid_from": "2026-06-08",
  "valid_until": null,
  "confidence": 0.92,
  "sensitivity": "low | medium | high",
  "status": "active | superseded | deleted",
  "supersedes": [],
  "tags": ["financial_planning", "response_style"]
}
```

### Minimal evaluation set

| Test | Prompt | Expected result |
|---|---|---|
| Recall | “What kind of financial advice do I prefer?” | Uses stored preference. |
| Relevance | “Explain F1 tire strategy.” | Does not inject financial memory. |
| Update | “Actually, use more detailed analysis going forward.” | Updates response-style memory. |
| Temporal | “What did I prefer before?” | Distinguishes old vs new preference. |
| Abstention | “What was my first boss’s name?” | Says it does not know if not stored. |
| Delete | “Forget my response-style preference.” | Stops using that memory. |

---

## 11. Key Takeaways

- Memory is not one thing. It includes storage, retrieval, summarization, updating, forgetting, governance, and reasoning.
- The simplest useful memory system is usually not a giant vector database; it is a disciplined process for deciding what should be remembered and when it should be used.
- The hardest problems are temporal change, stale memories, conflicting memories, privacy, and evaluation.
- Long context windows help, but they do not replace durable, governed, cross-session memory.
- For enterprise systems, memory needs provenance, controls, source-of-truth integration, and auditability.
- For coding and workflow agents, explicit file-based or project-scoped memory can be surprisingly effective because it is inspectable and versionable.

---

## 12. Reference Index

### Research

- A Survey on the Memory Mechanism of Large Language Model Based Agents — https://arxiv.org/abs/2404.13501
- MemGPT: Towards LLMs as Operating Systems — https://arxiv.org/abs/2310.08560
- LongMem: Augmenting Language Models with Long-Term Memory — https://arxiv.org/abs/2306.07174
- MemoryBank: Enhancing Large Language Models with Long-Term Memory — https://arxiv.org/abs/2305.10250
- Generative Agents — https://arxiv.org/abs/2304.03442
- LongMemEval — https://arxiv.org/abs/2410.10813
- LoCoMo — https://arxiv.org/abs/2402.17753
- Zep Temporal Knowledge Graph paper — https://arxiv.org/abs/2501.13956
- A-Mem — https://openreview.net/forum?id=FiM0M8gcct

### Open-source / developer projects

- Letta / MemGPT — https://github.com/letta-ai/letta
- Mem0 — https://github.com/mem0ai/mem0
- Zep — https://github.com/getzep/zep
- LongMem — https://github.com/Victorwz/LongMem
- MemoryBank SiliconFriend — https://github.com/zhongwanjun/MemoryBank-SiliconFriend
- LangGraph memory docs — https://docs.langchain.com/oss/python/concepts/memory
- LlamaIndex — https://github.com/run-llama/llama_index
- ReMe — https://github.com/agentscope-ai/ReMe
- Cline Memory Bank — https://docs.cline.bot/best-practices/memory-bank
- Awesome Memory for Agents — https://github.com/TsinghuaC3I/Awesome-Memory-for-Agents
- Agent Memory Paper List — https://github.com/Shichun-Liu/Agent-Memory-Paper-List

### Vendor / product docs

- OpenAI Memory FAQ — https://help.openai.com/en/articles/8590148-memory-faq
- OpenAI Reference Saved Memories — https://help.openai.com/en/articles/11146739-how-does-reference-saved-memories-work
- OpenAI Dreaming — https://openai.com/index/chatgpt-memory-dreaming/
- Google Vertex AI Memory Bank — https://cloud.google.com/blog/products/ai-machine-learning/vertex-ai-memory-bank-in-public-preview
- Google Gemini Enterprise Memory Bank — https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/memory-bank
- Anthropic Claude memory tool — https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool
- Claude Code memory — https://code.claude.com/docs/en/memory
- Microsoft 365 Copilot Memory — https://techcommunity.microsoft.com/blog/microsoft365copilotblog/introducing-copilot-memory-a-more-productive-and-personalized-ai-for-the-way-you/4432059
- Microsoft Copilot memory support — https://support.microsoft.com/en-us/microsoft-365-copilot/personalize-what-microsoft-365-copilot-remembers
- GitHub Copilot Memory — https://docs.github.com/en/copilot/how-tos/use-copilot-agents/copilot-memory
- AWS Bedrock AgentCore Memory — https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory.html
- Zep docs — https://help.getzep.com/concepts
- Mem0 research — https://mem0.ai/research
