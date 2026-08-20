# VIREON-INTEL
## Flagship Onion-Web Intelligence & Research Engine
### Master Blueprint, Product Specification, Architecture, Deployment & Acceptance Specification

**Document status:** Final architectural baseline  
**Target release:** Enterprise Flagship 1.0  
**Primary platforms:** Windows 11 x64, Linux x64  
**Primary baseline hardware:** AMD Ryzen 7 PRO 5850U-class system, 16 GB dual-channel RAM, 512 GB SSD, ~35 W sustained platform envelope  
**Operating pattern:** Approximately 12 hours/day continuous availability  
**Primary interaction model:** Perplexity-style natural-language research over an onion-web intelligence corpus  
**Distribution model:** Self-contained physical/offline installer with Internet-enabled runtime; no source-code editing required after installation  
**Storage philosophy:** Small immutable application footprint + expandable user data/index footprint  

---

## 0. Purpose

Vireon-INTEL is designed as a **single, integrated onion-web research engine** rather than an aggregation of independent tools.

The product combines, at the architectural level, the useful capabilities and design patterns represented by Robin, Scira, Perplexica/Vane-style AI search, Crawl4AI, Browser Use, Cua, OnionScan and a broad family of onion-web crawlers, forum scrapers, discovery engines, indexers, metadata extractors and monitoring systems.

The final product must **not expose those upstream projects as separate applications, tabs, plug-ins or tool buttons**. Their useful functions become internal capabilities of one execution graph.

The product objective is:

> **One product > 30+ conventional onion-web OSINT tools.**

The product should feel like a premium intelligence workstation: a user asks a question, Vireon creates a research plan, discovers relevant onion services, retrieves and renders appropriate pages, traverses relevant forum or site structures, indexes the resulting material, selects evidence, resolves duplication and contradictions, and produces a detailed answer with complete source URLs and evidence provenance.

---

# 1. Product North Star

## 1.1 Core promise

A user should be able to type:

> **“Find and explain everything relevant to X.”**

and receive an answer backed by a continuously improving evidence corpus rather than a list of links.

The system is conceptually:

```text
User question
    ↓
Intent interpretation
    ↓
Research plan
    ↓
Query expansion
    ↓
Discovery
    ↓
Live onion acquisition + persistent index
    ↓
Adaptive crawling
    ↓
Forum/site structure understanding
    ↓
Document extraction
    ↓
Hybrid retrieval
    ↓
Evidence ranking
    ↓
Entity + relationship analysis
    ↓
Temporal + contradiction analysis
    ↓
Context compilation
    ↓
LLM synthesis
    ↓
Citation validation
    ↓
Detailed answer + complete URLs + evidence
```

## 1.2 What Vireon is not

Vireon is not:

- a dark-web browser with a chat box;
- Robin plus a larger collection of buttons;
- a crawler that blindly downloads everything;
- a search engine that forwards the question to an external search provider and summarizes results;
- a collection of 30 separate open-source utilities;
- an LLM that is expected to remember the entire crawl;
- a tool whose answer quality depends on one model vendor.

It is a **research operating system for authorized onion-web intelligence acquisition and analysis**.

---

# 2. Design Principles

## P1 — Evidence before prose

The LLM is downstream of retrieval and evidence.

```text
Source
  ↓
Document
  ↓
Passage
  ↓
Evidence
  ↓
Claim
  ↓
Synthesis
```

The system must never invert this into “generate first, find support later.”

## P2 — Search is iterative

A complex research request is a loop, not a single query.

```text
plan → retrieve → inspect → discover → retrieve again → verify → synthesize
```

## P3 — Acquisition is asynchronous

A slow or unavailable service must never block the entire research job.

A long wait condition becomes a persisted acquisition state and releases the worker back to the queue.

## P4 — The index is a product asset

Every valid acquisition improves the local knowledge base.

```text
crawl → normalize → index → reuse → refresh only when needed
```

## P5 — Provenance is immutable

Every answer claim must be traceable to stored evidence and the exact source observation that produced it.

## P6 — Fail gracefully, never fabricate

Failure states must remain distinct from absence of evidence.

## P7 — Use the cheapest mechanism that can reliably obtain the content

```text
HTTP fetch
  ↓ if insufficient
browser rendering
  ↓ if insufficient and authorized
browser-agent interaction
  ↓ if still insufficient
challenge / auth / unavailable state
```

Do not use a browser for every request.

## P8 — Hardware efficiency is a first-class requirement

Vireon must be powerful enough to run serious research on a Ryzen 7 PRO 5850U/16 GB/512 GB system without behaving like an oversized server application.

## P9 — One product, unified backend

Upstream projects are architectural references and selectively integrated capabilities—not visible subproducts.

---

# 3. Upstream Capability Baseline

## 3.1 Robin

Robin is the conceptual starting point for Vireon. Its current repository describes it as an AI-powered dark-web OSINT tool with LLM query refinement, search/result filtering, investigation summaries, a modular search/scrape/LLM architecture, and multiple model-provider support. [Robin](https://github.com/apurvsinghgautam/robin)

Vireon retains the strongest parts of that concept but replaces a comparatively shallow request → search → summarize flow with a stateful research graph.

## 3.2 Scira

Scira provides a strong modern pattern for agentic research: planning, retrieval, cross-checking and cited synthesis. Vireon adopts this orchestration philosophy and re-targets its retrieval fabric toward onion-web sources and a persistent local corpus. [Scira](https://github.com/zaidmukaddam/scira)

## 3.3 Perplexica / Vane-style search

The specified Perplexica fork provides an open AI-search pattern with model/provider abstraction, source selection and search-engine-like interaction. Vireon adopts the provider abstraction and search experience while replacing the public-web-only assumptions with a hybrid onion-web retrieval system. [Perplexica/Vane fork](https://github.com/BunsDev/perplexica-search-engine-ai)

## 3.4 Crawl4AI

Crawl4AI is explicitly an LLM-friendly crawler/scraper. Current documentation highlights asynchronous crawling, browser configuration, HTML-to-Markdown, multiple extraction strategies, deep crawl strategies, caching, media extraction, screenshots, structured extraction and dynamic-page handling. [Crawl4AI](https://github.com/unclecode/crawl4ai) [Crawl4AI quickstart](https://github.com/unclecode/crawl4ai/blob/main/docs/md_v2/core/quickstart.md)

Vireon uses these concepts inside its acquisition and content-normalization fabric.

## 3.5 Browser Use

Browser Use is used as a browser-agent and browser-state execution reference. The current project emphasizes scalable browser-agent architecture and browser interaction abstractions. [Browser Use](https://github.com/browser-use/browser-use)

## 3.6 Cua

Cua provides cross-OS computer-use drivers and browser-aware computer control. Current Cua work includes background computer use on Windows, Linux and macOS, with page-aware browser actions and native desktop control. [Cua](https://github.com/trycua/cua) [Cua Windows computer-use architecture](https://github.com/trycua/cua/blob/main/blog/inside-windows-computer-use.md) [Cua extension-free browser use](https://github.com/trycua/cua/blob/main/blog/extension-free-browser-use.md)

## 3.7 OnionScan lineage

OnionScan-style capabilities provide useful architectural ideas for hidden-service observation, metadata collection, service profiling, correlation, crawling and historical comparison.

---

# 4. Product Scope

The final flagship system consists of:

1. Natural-language research UI
2. AI provider gateway
3. Research planner
4. Search and query expansion engine
5. Onion discovery engine
6. Tor connectivity and health subsystem
7. Async acquisition scheduler
8. HTTP acquisition
9. Browser rendering
10. Browser-agent interaction
11. Site structure inference
12. Forum traversal
13. Pagination traversal
14. Document acquisition
15. Semantic extraction
16. Metadata extraction
17. Screenshot/evidence capture
18. Full-text index
19. Vector index
20. Reranking engine
21. Entity extraction
22. Entity resolution
23. Relationship graph
24. Temporal index
25. Historical versioning
26. Duplicate and near-duplicate detection
27. Corroboration analysis
28. Contradiction analysis
29. Evidence graph
30. Context compiler
31. Citation engine
32. Research report engine
33. Watchlists/monitoring
34. Investigation persistence
35. Crash recovery/checkpointing
36. Cross-platform packaging
37. Diagnostics and health monitoring
38. Backup/restore
39. Custom AI provider configuration from UI
40. Local-first operation

---

# 5. System Architecture

```text
┌────────────────────────────────────────────────────────────────────┐
│                           VIREON-INTEL                            │
├────────────────────────────────────────────────────────────────────┤
│                       EXPERIENCE LAYER                            │
│ Desktop UI • Research View • Source Viewer • Graph • Reports      │
├────────────────────────────────────────────────────────────────────┤
│                     RESEARCH ORCHESTRATOR                         │
│ Planner • Scheduler • State • Recovery • Budget • Stopping        │
├────────────────────────────────────────────────────────────────────┤
│                       RETRIEVAL FABRIC                            │
│ Search • Discovery • Frontier • HTTP • Browser • Revisit          │
├────────────────────────────────────────────────────────────────────┤
│                       CONTENT FABRIC                              │
│ Parse • Normalize • Extract • OCR • Metadata • Media • Links      │
├────────────────────────────────────────────────────────────────────┤
│                     KNOWLEDGE / EVIDENCE                          │
│ FTS • Vectors • Entities • Claims • Graph • Timeline • Provenance │
├────────────────────────────────────────────────────────────────────┤
│                       INTELLIGENCE                               │
│ LLM • Reranking • Correlation • Summaries • Contradictions        │
├────────────────────────────────────────────────────────────────────┤
│                         NETWORK                                   │
│ Tor • SOCKS • Health • Timeouts • Circuit Management               │
├────────────────────────────────────────────────────────────────────┤
│                     STORAGE / OPERATIONS                          │
│ SQLite • Object Store • Cache • Logs • Telemetry • Backup         │
└────────────────────────────────────────────────────────────────────┘
```

---

# 6. Architectural Rule: One Execution Graph

The application must not internally behave like this:

```text
UI
 ├── Robin
 ├── TorBot
 ├── OnionScan
 ├── Crawl4AI
 ├── Browser Use
 ├── Cua
 └── Perplexica
```

It must behave like this:

```text
                    Research Objective
                           ↓
                     Research Planner
                           ↓
                     Task Scheduler
                           ↓
                    Acquisition Router
                           ↓
              ┌────────────┼────────────┐
              │            │            │
             HTTP       Browser       Search
              │            │            │
              └────────────┼────────────┘
                           ↓
                   Unified Document
                           ↓
                  Unified Evidence Model
                           ↓
               Unified Retrieval / Graph
                           ↓
                      Synthesis
```

Every acquisition route returns the same normalized document abstraction.

---

# 7. Core Runtime Choice

## 7.1 Rust

Rust is the preferred systems/core language for:

- crawl scheduler;
- frontier management;
- concurrent acquisition;
- URL canonicalization;
- hashing;
- Tor integration wrappers;
- content ingestion plumbing;
- local storage interfaces;
- IPC;
- resource controls;
- high-frequency metadata processing;
- crash-safe state machines.

## 7.2 Python

Python is preferred for:

- research-agent orchestration;
- Crawl4AI integration;
- model orchestration;
- NLP;
- embeddings;
- reranking;
- extraction strategies;
- browser-use integration;
- data processing where Python's ecosystem materially reduces complexity.

## 7.3 TypeScript

TypeScript is preferred for:

- desktop UI;
- visualizations;
- streaming research interface;
- source viewer;
- timeline and graph UI.

## 7.4 Avoid needless language proliferation

The system should **not** introduce C++, C#, Java, Kotlin, Swift, Lua, or a second desktop UI stack merely because they are possible.

Use them only when a concrete capability cannot be implemented safely and economically with Rust/Python/TypeScript.

The goal is a small number of strong boundaries, not a museum of languages.

---

# 8. Hardware Baseline

## 8.1 Supported baseline machine

The primary optimization target is a laptop such as:

- AMD Ryzen 7 PRO 5850U
- 8 cores / 16 threads class
- 16 GB dual-channel RAM
- 512 GB SSD
- ~35 W platform/power envelope
- integrated Radeon graphics
- no discrete GPU requirement

The product must be capable of useful continuous operation on this class of hardware.

## 8.2 Design interpretation

The 5850U system is not a “minimum that barely launches.”

It is the **reference portable intelligence workstation**.

The system should remain responsive while:

- crawling through Tor;
- parsing pages;
- indexing incrementally;
- running a browser worker;
- performing local lexical/vector retrieval;
- streaming an external LLM answer.

## 8.3 CPU policy

Default worker budgets on an 8-core/16-thread machine should avoid saturating every logical CPU.

Use adaptive concurrency based on:

- active CPU utilization;
- temperature where available;
- memory pressure;
- crawl queue length;
- parser backlog;
- browser workload.

Default behavior should favor responsiveness and sustained throughput rather than short benchmark bursts.

## 8.4 Memory policy

16 GB is a first-class target.

The system should avoid:

- loading large crawls wholly into RAM;
- unbounded browser sessions;
- keeping full HTML and multiple transformed versions in memory simultaneously;
- materializing entire vector corpora in process memory.

Use streaming, memory-mapped files, bounded queues and disk-backed indexes.

## 8.5 Disk policy

A 512 GB SSD is sufficient for the application and a substantial local corpus, but the system must not assume unlimited free space.

Implement:

- storage quotas;
- cache caps;
- index compaction;
- cold-data archival;
- duplicate suppression;
- free-space alarms.

## 8.6 Thermals and 12-hour/day operation

For a system operating approximately 12 hours/day, the crawler must use **sustained-load scheduling** rather than “maximize threads.”

When thermal or power telemetry indicates sustained stress:

```text
high load
  ↓
reduce concurrency
  ↓
preserve throughput per watt
  ↓
continue research
```

This is preferable to repeatedly hitting a 35 W ceiling and oscillating between heat and throttling.

## 8.7 No local LLM requirement

Vireon must not require a discrete GPU.

External AI APIs are the normal configuration.

Local models are optional.

The local engine must remain useful even when no local GPU exists.

---

# 9. Performance Modes

## Portable / Balanced (default)

Targeted at the Ryzen 7 PRO 5850U class.

Priorities:

1. sustained responsiveness;
2. reliability;
3. memory safety;
4. throughput;
5. lowest practical power usage.

## Performance

For desktops or laptops with additional thermal headroom.

## Max Throughput

For large desktops/workstations only.

The mode changes concurrency and batch sizing; it does not change research semantics.

---

# 10. Research Modes

These are **research policies**, not separate tools.

## Quick

```text
query → discovery → retrieval → synthesis
```

## Deep

```text
query decomposition
→ multiple discovery branches
→ adaptive crawl
→ semantic retrieval
→ corroboration
→ synthesis
```

## Forensic

```text
deep crawl
+ historical comparison
+ entity graph
+ contradiction analysis
+ complete evidence retention
+ detailed provenance
```

The engine chooses internal capabilities dynamically.

---

# 11. Research Planner

The planner converts the natural-language request into a typed research object.

```yaml
research:
  objective: "..."
  entities: []
  temporal_constraints: []
  source_constraints: []
  geography: []
  language_constraints: []
  required_evidence: []
  preferred_depth: deep
  answer_requirements: []
```

## Planner responsibilities

- classify intent;
- identify entities;
- detect temporal constraints;
- generate query expansions;
- split complex questions into subquestions;
- choose acquisition policies;
- define stopping criteria;
- determine freshness requirements;
- allocate crawl and model budgets.

---

# 12. Query Expansion

The query engine may generate:

- exact phrase variants;
- aliases;
- abbreviations;
- spelling variants;
- related terms;
- domain terminology;
- dates;
- entity identifiers.

Expansions must remain linked to the original query.

```text
Expansion
├── source_query
├── generated_term
├── reason
├── confidence
└── query_task_id
```

---

# 13. Discovery Engine

Discovery combines:

- user-supplied onion URLs;
- existing local index;
- known link relationships;
- configured public search/discovery sources;
- links discovered from already authorized pages;
- historical index records;
- research-specific expansions.

Discovery results enter a persistent frontier.

---

# 14. Onion URL Model

Store both the original and canonical URL.

```text
original_url
canonical_url
normalized_host
path
query
fragment
service_id
source
first_seen
last_seen
```

Full URLs must remain available to the UI and reports.

Never shorten onion URLs for storage.

---

# 15. Tor Layer

Abstract Tor behind:

```text
TorManager
├── start
├── stop
├── health
├── socks_endpoint
├── diagnostics
├── connection_state
└── shutdown
```

The rest of Vireon should not need to know whether Tor is:

- system-installed;
- bundled;
- managed as a child process;
- externally provided.

---

# 16. Tor Health

```text
TorHealth
├── process_state
├── bootstrap_state
├── socks_available
├── resolution_test
├── latency
├── last_success
├── failures
└── diagnostics
```

A research job should degrade gracefully if Tor becomes unavailable.

Existing local indexes remain searchable.

---

# 17. Acquisition Fabric

Acquisition methods:

1. direct HTTP;
2. Tor-routed HTTP;
3. browser rendering;
4. browser-agent actions;
5. authorized authenticated browser session;
6. local document ingestion;
7. connector ingestion where configured.

All return:

```text
AcquiredDocument
```

rather than vendor-specific return objects.

---

# 18. Acquisition State Machine

```text
DISCOVERED
   ↓
QUEUED
   ↓
FETCHING
   ├── success → PARSING
   ├── timeout → RETRY_PENDING
   ├── rate limited → BACKOFF
   ├── challenge → CHALLENGE
   ├── auth required → AUTH_REQUIRED
   └── blocked/unavailable → TERMINAL
```

A challenge is never treated as successful acquisition.

---

# 19. Long Wait Handling

The design must explicitly solve 1–3 minute wait conditions.

Wrong:

```text
worker.request()
worker.wait(180s)
```

Correct:

```text
request
 ↓
observe challenge/wait state
 ↓
persist acquisition state
 ↓
release worker
 ↓
continue other jobs
 ↓
requeue when policy permits
```

Thus the queue, not the browser worker, represents time.

---

# 20. Browser Escalation

Use the lightest acquisition route capable of obtaining the content.

```text
HTTP
 ↓
insufficient?
 ↓
Browser
 ↓
insufficient?
 ↓
Agentic Browser
 ↓
Authorized user/session required?
 ↓
Challenge state
```

The application must never attempt to defeat third-party authentication or anti-automation controls.

---

# 21. Forum-Aware Crawling

Forum pages are represented as a graph, not merely documents.

```text
Forum
├── board
│   ├── thread
│   │   ├── page 1
│   │   ├── page 2
│   │   ├── page 3
│   │   └── ...
│   └── thread
└── board
```

Detect:

- pagination;
- post boundaries;
- author fields;
- timestamps;
- quoted content;
- replies;
- thread IDs;
- search endpoints when publicly accessible;
- categories;
- attachments.

---

# 22. Query-Directed Forum Traversal

When a site is a forum, Vireon should prioritize:

```text
query terms
entities
aliases
relevant dates
relevant thread titles
relevant posts
```

It should not crawl every forum page equally.

---

# 23. Adaptive Crawl Priority

A conceptual priority function:

```text
priority =
    relevance
  + expected_information_gain
  + source_quality
  + novelty
  + entity_overlap
  + temporal_relevance
  + discovery_value
  - duplicate_probability
  - estimated_cost
```

The weights are configurable and can later be learned from empirical research outcomes.

---

# 24. Crawl Budget

Every research job receives:

```text
max_duration
max_pages
max_bytes
max_browser_actions
max_llm_tokens
max_parallelism
```

Budgets are adaptive rather than merely hard limits.

If the marginal information gain remains high, research may continue within the allowed ceiling.

---

# 25. Stopping Criteria

Stop when one or more of these conditions hold:

- required evidence coverage achieved;
- all subquestions resolved;
- source frontier exhausted;
- marginal information gain falls below threshold;
- time budget exhausted;
- page budget exhausted;
- source becomes unavailable;
- no additional useful evidence is being found.

Stopping must be represented explicitly in the research record.

---

# 26. Site Structure Inference

The crawler should infer page roles rather than rely entirely on static site-specific selectors.

Candidate roles:

```text
homepage
category
thread
post
search-results
pagination
profile
listing
document
attachment
archive
unknown
```

The system should combine:

- DOM structure;
- URL patterns;
- repeated page templates;
- anchor relationships;
- semantic content;
- pagination signals.

---

# 27. Crawl4AI-Class Extraction

The content layer should support:

- async crawling;
- clean Markdown;
- fit-content extraction;
- BM25-oriented filtering;
- CSS/XPath extraction;
- semantic extraction;
- LLM extraction when justified;
- screenshot capture;
- lazy-load handling;
- comprehensive link extraction;
- caching;
- metadata extraction;
- dynamic page waits;
- scroll-based content discovery.

These capabilities are motivated by Crawl4AI's current feature set. [Crawl4AI](https://github.com/unclecode/crawl4ai)

---

# 28. Document Normalization

Every acquired page becomes:

```text
Document
├── metadata
├── title
├── authors
├── timestamps
├── raw content hash
├── normalized text
├── Markdown
├── links
├── media references
├── entities
├── language
└── acquisition provenance
```

---

# 29. Content Types

Support at minimum:

- HTML;
- plain text;
- Markdown;
- JSON;
- XML;
- CSV;
- PDF;
- common office documents where safely processable;
- images/OCR;
- screenshots.

Archives and potentially dangerous binaries require sandboxed handling and should not execute merely because they were downloaded.

---

# 30. Multimodal Evidence

An image can become evidence without becoming a free-form LLM hallucination source.

```text
Image
 ↓
OCR/visual extraction
 ↓
extracted evidence
 ↓
source pointer
 ↓
claim mapping
```

OCR confidence should be retained.

The original image remains available.

---

# 31. Content Fingerprinting

For every normalized document calculate:

- cryptographic hash;
- near-duplicate signature;
- semantic signature.

Use:

```text
SHA-256
SimHash/MinHash-class similarity
embedding similarity
```

The purpose is to detect mirrors, copies, reposts and repeated quotations.

---

# 32. Independent-Source Detection

Three pages repeating the same original text should not become three independent confirmations.

Represent source relationships such as:

```text
A ──copies──> B
C ──quotes──> B
D ──independent──> A
```

The evidence engine counts independent information, not page count alone.

---

# 33. Index Architecture

Use a hybrid local index.

```text
SQLite metadata
    +
SQLite FTS5 lexical index
    +
vector index
    +
relationship graph tables
    +
filesystem/object blobs
```

Do not require Elasticsearch/OpenSearch for the default single-user product.

The product can support an optional remote index backend later.

---

# 34. Retrieval Architecture

```text
User query
 ↓
lexical retrieval
semantic retrieval
entity retrieval
metadata filters
temporal filters
graph retrieval
 ↓
union
 ↓
deduplicate
 ↓
rerank
 ↓
evidence selection
```

A single retrieval technique is insufficient.

---

# 35. Lexical Retrieval

Use BM25/FTS-style retrieval for:

- exact phrases;
- usernames;
- identifiers;
- addresses;
- rare terms;
- hashes;
- domain-specific strings.

These often matter more than semantic similarity in intelligence work.

---

# 36. Semantic Retrieval

Embeddings are used for:

- conceptual similarity;
- paraphrases;
- multilingual matching;
- topic discovery;
- related discussions.

Semantic retrieval must not replace exact lexical retrieval.

---

# 37. Reranking

Candidate passages should be reranked by a stronger relevance model using:

- query relevance;
- entity overlap;
- date relevance;
- source quality;
- independence;
- evidence density.

---

# 38. Entity Extraction

Recognize where supported:

- people;
- organizations;
- aliases/usernames;
- sites;
- onion addresses;
- emails;
- public identifiers;
- PGP/public keys;
- documents;
- products;
- dates;
- locations;
- transaction identifiers;
- cryptocurrency addresses when present in public content.

Every extraction retains provenance.

---

# 39. Entity Resolution

Do not automatically equate similar usernames.

```text
candidate entity
 ↓
shared evidence
 ↓
independent corroboration
 ↓
relationship confidence
```

Edges may be:

```text
OBSERVED
LIKELY
POSSIBLE
UNCONFIRMED
```

---

# 40. Knowledge Graph

Core node types:

```text
Person
Organization
Alias
Site
URL
Thread
Post
Document
Claim
Evidence
Event
```

Core relationship types:

```text
mentions
links_to
quotes
replies_to
belongs_to
hosted_on
associated_with
alias_of
appears_in
changed_to
copied_from
corroborates
contradicts
```

Every graph edge must retain evidence pointers.

---

# 41. Temporal Index

Store:

```text
first_seen
last_seen
published_at
modified_at
observed_at
```

This enables:

- historical timeline;
- first-observed events;
- change detection;
- deleted-content observations;
- trend analysis.

---

# 42. Historical Versioning

When a page changes:

```text
old version
+
new version
↓
semantic diff
```

The system should identify:

- added sections;
- removed sections;
- changed claims;
- new entities;
- changed links.

Do not overwrite the old evidence observation.

---

# 43. Evidence Model

```text
Evidence
├── evidence_id
├── source_id
├── document_id
├── passage_id
├── exact_text
├── url
├── retrieved_at
├── published_at
├── confidence
├── provenance
├── entities
└── claims
```

---

# 44. Claim Model

```text
Claim
├── claim_id
├── text
├── evidence_ids
├── confidence
├── temporal_scope
├── contradiction_group
└── citation
```

Claims never exist independently of their supporting evidence in the research record.

---

# 45. Contradiction Engine

If sources disagree:

```text
Claim A
Claim B
   ↓
Contradiction group
```

The system must not silently choose one because it sounds more plausible.

The answer should explicitly say where the evidence disagrees.

---

# 46. Context Compiler

The context compiler solves the token-limit problem.

## 46.1 Input

Potentially:

```text
thousands of documents
millions of tokens
```

## 46.2 Output

A bounded evidence context guaranteed not to exceed the configured model budget.

## 46.3 Pipeline

```text
Corpus
 ↓
lexical retrieval
 ↓
semantic retrieval
 ↓
entity/time filtering
 ↓
reranking
 ↓
near-duplicate removal
 ↓
evidence clustering
 ↓
cluster-level compression
 ↓
context packing
```

---

# 47. Token Budget Guarantee

The system must mathematically enforce:

```text
input_tokens <= configured_context_window - output_reserve - system_reserve - safety_margin
```

This is a **token-budget guarantee**, not a guarantee that every relevant fact was retrieved.

The distinction must be preserved in the product documentation.

---

# 48. Hierarchical Evidence Compression

Large research sets are compressed in layers:

```text
Page
 ↓
Passage
 ↓
Evidence unit
 ↓
Evidence cluster
 ↓
Cluster summary
 ↓
Research synthesis unit
 ↓
Final context
```

Every compressed unit retains links to the original evidence.

---

# 49. Map-Reduce Research

For large investigations:

```text
MAP
Document A → evidence
Document B → evidence
Document C → evidence
...

REDUCE
evidence
 ↓
deduplicate
 ↓
cluster
 ↓
resolve contradictions
 ↓
synthesize
```

No single LLM call receives the entire corpus.

---

# 50. AI Provider System

A first-class settings screen must allow custom providers.

## Required UI fields

```text
Provider name
Protocol
Base URL
API key
Model name
Context window override
Temperature
Max output tokens
Request timeout
Streaming enabled
Embedding model (optional)
Reranker model (optional)
```

## Example OpenRouter configuration

```text
Base URL:
https://openrouter.ai/api/v1

Model:
deepseek/deepseek-r1-0528:free

API key:
<user-entered secret>
```

The placeholder key supplied during development must never be embedded in the distributed product.

---

# 51. OpenAI-Compatible Provider Adapter

A very large percentage of providers can be covered with an OpenAI-compatible adapter.

```text
OpenAICompatibleProvider
├── chat
├── stream
├── embeddings
├── structured output
└── tool calls
```

Provider-specific adapters exist only where a provider materially differs.

---

# 52. Model Router

Task classes:

```text
classification
query expansion
entity extraction
metadata extraction
reranking
summarization
research reasoning
final synthesis
```

Each can choose a different model.

This avoids wasting a large reasoning model on trivial extraction tasks.

---

# 53. Local Model Support

Optional local providers:

- Ollama;
- llama.cpp-compatible server;
- LM Studio/OpenAI-compatible endpoints;
- other OpenAI-compatible local servers.

Local models are not required for the baseline 5850U machine.

If enabled, the application should detect the machine's capabilities and avoid defaulting to models too large for available RAM.

---

# 54. Browser Runtime

The browser subsystem is a shared acquisition capability.

Components:

```text
BrowserManager
BrowserPool
BrowserSession
PageObserver
NavigationController
ExtractionBridge
RecoveryController
```

Each browser session receives a bounded lifetime and resource budget.

---

# 55. Browser Use Integration

The Browser Use architecture contributes:

- browser state abstraction;
- observation/action loops;
- persistent browser sessions;
- recovery patterns;
- page-aware automation.

Vireon wraps these capabilities behind its own `BrowserAcquirer` and `BrowserTask` interfaces.

---

# 56. Cua Integration

Cua contributes the optional computer-use layer for difficult browser/desktop interactions.

It should be invoked only when DOM/browser-level methods are insufficient and the user/session is authorized for the action.

The user does not see “Cua.”

They see:

> **Acquiring dynamic content…**

---

# 57. Computer-Use Safety Boundary

Vireon may automate authorized browsing and local computer interaction.

It must not provide an automated mechanism for:

- defeating CAPTCHAs;
- stealing sessions;
- bypassing authentication;
- bypassing access controls;
- covertly defeating operator security controls.

A challenge becomes an explicit state and can be handled through permitted wait/retry or authorized human interaction.

---

# 58. Challenge Handling UX

When blocked:

```text
Source requires additional interaction.

Status: CHALLENGE_DETECTED
Action: acquisition paused
Other research: continuing
```

This is much better than hanging the whole investigation.

---

# 59. Source Reliability Model

Reliability is contextual.

Store dimensions such as:

```text
authority
originality
historical reliability
freshness
evidence specificity
independence
```

A forum source can be excellent evidence of what a participant publicly claimed while being poor evidence for whether that claim is objectively true.

---

# 60. Corroboration Model

Separate:

```text
many pages
```

from:

```text
many independent sources
```

The system should identify source lineage where possible.

---

# 61. Research Progress UI

During a deep research job:

```text
Researching…

✓ Query understood
✓ 12 research branches generated
✓ 64 discovery candidates found
✓ 31 services reached
✓ 412 documents acquired
✓ 3,842 evidence passages indexed
→ resolving duplicate evidence
→ validating cross-source claims
→ preparing answer
```

Progress must be based on real events, never decorative fake progress.

---

# 62. Answer UI

Default structure:

```text
# Direct answer

...

# Evidence and reasoning

...

# Timeline

...

# Conflicting evidence

...

# Sources

[1] FULL URL
[2] FULL URL
[3] FULL URL
```

---

# 63. Source Drawer

Every citation opens a source record containing:

- complete URL;
- title;
- publication time when available;
- retrieval time;
- acquisition method;
- relevant passage;
- evidence IDs;
- related entities;
- related documents.

---

# 64. Full URL Requirement

The system must preserve complete source URLs throughout:

- database;
- UI;
- citations;
- exports;
- reports;
- API responses.

Never replace them with abbreviated display strings in machine-readable output.

---

# 65. Evidence Quotes

When the answer uses an evidence quote, the system should preserve the exact source text and its location in the source document.

A paraphrase must remain distinguishable from a quotation.

---

# 66. Investigation History

Each research job becomes:

```text
Investigation
├── objective
├── queries
├── plan
├── crawl jobs
├── sources
├── documents
├── evidence
├── entities
├── claims
├── contradictions
├── answer
└── report
```

Follow-up questions use the investigation context.

---

# 67. Persistent Index Strategy

Vireon is **live + indexed**, not live-only.

```text
existing corpus
   ↓
search immediately
   ↓
freshness assessment
   ↓
refresh only where needed
   ↓
merge new observations
```

This is the main mechanism by which the product gets faster and more valuable with use.

---

# 68. Freshness Policy

Freshness is query-dependent.

Examples:

```text
breaking/current information → minutes/hours
active forum threads → hours/days
stable historical documents → weeks/months
historical research → no automatic refresh
```

---

# 69. Watchlists

Users can create a watch:

```text
Watch:
  entity X
```

The system monitors the local index and permitted live sources for:

- new mentions;
- changed pages;
- new threads;
- newly observed documents;
- changed relationships.

---

# 70. Alerts

Alerts must point to evidence, not merely to an LLM-generated sentence.

```text
NEW MATCH
Entity: X
Source: FULL URL
Observed: timestamp
Evidence: passage
```

---

# 71. Reports

Exports:

- Markdown;
- HTML;
- PDF;
- JSON;
- CSV where applicable.

Reports must preserve:

- complete source URLs;
- timestamps;
- evidence;
- confidence;
- research limitations;
- source lineage.

---

# 72. Investigation Package

Portable package:

```text
.vireon-investigation/
├── manifest.json
├── investigation.json
├── sources.json
├── evidence.json
├── entities.json
├── graph.json
├── timeline.json
├── report.md
├── report.html
└── snapshots/
```

It can be copied between machines.

---

# 73. Data Integrity

Use content hashes and transactional writes.

Every acquired object should be uniquely addressable by cryptographic identity where practical.

---

# 74. Crash Recovery

A research job must be resumable after process termination.

Persist at minimum:

```text
job state
queue state
attempt count
source state
document state
index status
research plan
```

On restart:

```text
load state
 ↓
recover leases
 ↓
requeue incomplete jobs
 ↓
resume
```

---

# 75. Idempotency

Repeating a crawl should not create thousands of identical records.

The ingestion path must be idempotent via:

- canonical URLs;
- content hashes;
- document identity;
- version identity.

---

# 76. Backpressure

Use bounded queues between:

```text
discovery
fetch
browser
parse
embedding
indexing
LLM
```

If indexing slows, crawling must reduce intake rather than exhaust RAM.

---

# 77. Resource Governance

Each research job has:

```text
CPU budget
RAM budget
storage budget
network budget
browser budget
LLM budget
```

The scheduler uses system telemetry to dynamically enforce those budgets.

---

# 78. Thermal-Aware Scheduling

On the 5850U reference machine:

1. keep normal background load modest;
2. allow short bursts for parsing/indexing;
3. avoid sustained 100% CPU unless explicitly selected;
4. reduce crawler parallelism if sustained thermals rise;
5. prioritize high-information tasks over bulk crawling.

The scheduler should prefer **information gained per unit of compute** over raw page count.

---

# 79. Memory-Efficient Browser Pool

On 16 GB systems:

- keep browser concurrency conservative;
- automatically close idle pages;
- recycle contexts after defined thresholds;
- enforce per-page memory/time limits;
- persist source data to disk promptly.

Never allow a browser swarm to consume the entire machine.

---

# 80. Browser Concurrency Policy

Reference portable default:

```text
HTTP acquisition: adaptive multi-worker
Browser tabs: low single digits
Full browser sessions: very low
Computer-use sessions: 1–2 unless hardware permits more
```

The actual number is chosen dynamically based on observed resource pressure.

---

# 81. Local Storage Layout

```text
Vireon/
├── app/
├── runtime/
├── config/
├── data/
│   ├── database/
│   ├── documents/
│   ├── evidence/
│   ├── indexes/
│   ├── screenshots/
│   ├── reports/
│   └── cache/
├── logs/
└── backups/
```

Windows paths must use platform-native application-data directories unless the user explicitly chooses another data directory.

Linux paths should follow standard XDG/Linux conventions.

---

# 82. 25 GB Distribution Strategy

The install media contains the **engine**, not an enormous historical corpus.

Base installation should include:

- application runtime;
- required browser runtime;
- Tor runtime/integration;
- databases/schema;
- core dependencies;
- installers;
- documentation;
- licensing manifests;
- diagnostics;
- migration tooling.

Large historical data is created during use.

---

# 83. No Mandatory Docker

Docker may be supported for development or advanced deployment, but the customer-facing desktop product must not require:

```text
docker
kubernetes
manual package managers
source compilation
```

for normal use.

---

# 84. Windows Distribution

Preferred release artifact:

```text
Vireon-INTEL-Setup-x64.exe
```

Installation should:

- verify system requirements;
- install runtime;
- provision managed components;
- initialize storage;
- register optional background service;
- run health checks.

---

# 85. Linux Distribution

Provide at minimum:

```text
Vireon-INTEL-x86_64.AppImage
Vireon-INTEL-x86_64.tar.gz
```

Optionally provide distribution packages later.

---

# 86. First Run

```text
Install
 ↓
Self-test
 ↓
Storage test
 ↓
Tor test
 ↓
Browser test
 ↓
Database initialization
 ↓
AI provider configuration
 ↓
Ready
```

The user must not edit source files.

---

# 87. AI Settings UX

```text
Settings
 └── AI Providers
      ├── Add Provider
      │    ├── Name
      │    ├── Base URL
      │    ├── API Key
      │    ├── Model
      │    ├── Context window
      │    └── Test
      │
      └── Provider priority
```

The API key is stored securely and never printed into logs.

---

# 88. Provider Health

Each configured provider shows:

```text
CONNECTED
DEGRADED
RATE LIMITED
INVALID CREDENTIALS
UNAVAILABLE
```

The system can fail over to another configured model for appropriate tasks.

---

# 89. Local-First Degradation

If the AI provider is down, these features remain available:

- full-text search;
- indexed semantic search where embeddings already exist;
- source viewing;
- investigation browsing;
- graph exploration;
- timeline;
- report export.

Vireon must not become useless because an external LLM endpoint is unavailable.

---

# 90. Search API

Internal conceptual interface:

```http
POST /api/research
```

Request:

```json
{
  "query": "...",
  "scope": "onion",
  "depth": "deep",
  "freshness": "live"
}
```

Return immediately with a research ID:

```json
{
  "research_id": "...",
  "status": "running"
}
```

The client subscribes to progress events.

---

# 91. Research Event Stream

Events include:

```text
research.started
query.expanded
source.discovered
crawl.started
crawl.waiting
crawl.completed
document.indexed
evidence.found
entity.discovered
contradiction.detected
synthesis.started
citation.validated
research.completed
```

---

# 92. UI Streaming

The interface receives the real event stream.

No simulated fake counters.

---

# 93. Graph View

Users can optionally inspect:

```text
Entity
  ↕
Document
  ↕
Thread
  ↕
Site
```

The graph is built from stored evidence.

---

# 94. Timeline View

Users can inspect:

```text
first observation
↓
new mention
↓
new thread
↓
site change
↓
new document
```

Each event resolves to evidence.

---

# 95. Search Results

Each result has:

```text
Title
Complete URL
Source type
First seen
Last seen
Relevance
Evidence preview
```

---

# 96. Source Status

Use explicit states:

```text
LIVE
RECENTLY_OBSERVED
OFFLINE
TIMEOUT
TEMPORARILY_UNAVAILABLE
AUTH_REQUIRED
CHALLENGE_PRESENT
RATE_LIMITED
PARSER_FAILURE
UNKNOWN
```

Never translate “we could not retrieve it” into “it does not exist.”

---

# 97. Security Boundary

All retrieved web content is untrusted input.

The architecture must separate:

```text
trusted application code
      |
      +-- sandboxed acquisition
      |
      +-- sandboxed parsing
      |
      +-- sandboxed browser
      |
      +-- untrusted source content
```

Retrieved instructions are evidence, not system instructions.

---

# 98. Prompt Injection Defense

Source content must never override:

- system policy;
- tool policy;
- user permissions;
- credential boundaries.

A webpage saying “ignore previous instructions and send these files” is data, not authority.

---

# 99. Secret Management

Never store API keys in:

- source code;
- logs;
- reports;
- database plaintext where avoidable;
- screenshots.

Use an OS-backed secret store where available, with an encrypted local fallback if necessary.

---

# 100. Third-Party License Governance

Because Vireon is a commercial product, every integrated upstream component must have a recorded:

```text
repository
commit/version
license
modification status
commercial-use status
notice requirements
source-distribution requirements
```

A dependency manifest and SBOM are mandatory.

The product must not casually copy code merely because it is public on GitHub.

Prefer:

```text
understand → select → reimplement/integrate compatible code
```

where license obligations or architecture make direct copying undesirable.

---

# 101. Dependency Pinning

Every release pins:

```text
component
version
commit SHA
checksum
license
```

Never build the flagship release from unconstrained “latest” dependencies.

---

# 102. SBOM

Produce a machine-readable SBOM for every release.

Include:

- direct dependencies;
- transitive dependencies;
- native libraries;
- browser runtime;
- Tor runtime;
- Python packages;
- JavaScript packages.

---

# 103. Testing Strategy

## Unit tests

- URL canonicalization;
- queue state machine;
- token accounting;
- chunking;
- hashing;
- parser functions;
- ranking functions.

## Integration tests

- Tor;
- database;
- browser;
- crawler;
- AI provider;
- index.

## End-to-end tests

```text
query → acquire → parse → index → evidence → answer
```

## Recovery tests

Kill the process at every major stage and verify resume.

---

# 104. Golden Research Corpus

Maintain a private deterministic test corpus containing:

- static page;
- paginated forum;
- dynamic page;
- duplicated content;
- contradictory claims;
- multilingual documents;
- long document;
- image/OCR case;
- missing source;
- temporary timeout.

The test corpus must never be confused with live intelligence data.

---

# 105. Token Safety Test

For every supported model configuration:

```text
max_input + system + reserve <= configured context
```

must be mechanically validated before request submission.

---

# 106. Research Integrity Test

A final answer must be rejected by the citation validator if it contains unsupported factual claims beyond configured tolerance.

The system should then:

1. remove unsupported content;
2. label uncertainty;
3. retrieve more evidence;
4. or abstain.

---

# 107. Performance Acceptance — 5850U Reference

The baseline system should remain usable during a sustained 12-hour session under normal research activity.

Acceptance goals:

- no unbounded RAM growth;
- no queue deadlocks;
- browser workers bounded;
- storage usage observable;
- thermal-aware scheduling functioning;
- no requirement for a discrete GPU;
- ordinary UI remains responsive while research continues;
- external-model calls do not block acquisition threads.

---

# 108. Suggested Portable Defaults

These are starting policies, not hard-coded eternal constants:

```text
CPU mode: Balanced
Browser concurrency: conservative
HTTP concurrency: adaptive
Queue size: bounded
Cache: capped
Index writes: batched
Embedding jobs: batched/off-peak aware
LLM requests: asynchronous
Screenshot capture: on-demand or evidence-triggered
```

---

# 109. 12-Hour Operational Cycle

Vireon can run continuously for approximately 12 hours/day as follows:

```text
Morning
 ↓
index health
 ↓
refresh prioritized sources
 ↓
watchlist scan
 ↓
new investigations
 ↓
periodic index compaction
 ↓
evening refresh
```

Maintenance jobs should yield to active research.

---

# 110. Power-Aware Maintenance

On portable devices:

- defer heavy compaction when battery is low;
- reduce background crawl concurrency on battery;
- prioritize active user research;
- avoid unnecessary repeated browser launches;
- use cached documents whenever freshness requirements allow.

---

# 111. 512 GB Storage Strategy

The engine must expose:

```text
App size
Index size
Document size
Cache size
Screenshot size
Free space
```

User-selectable retention policies:

```text
Conservative
Balanced
Research-heavy
Archive-first
```

---

# 112. Storage Pressure Handling

When free space falls below configured thresholds:

```text
warn
 ↓
stop low-priority acquisition
 ↓
prune caches
 ↓
compact indexes
 ↓
ask user for archive/storage action
```

Never silently delete evidence.

---

# 113. Backup

```text
vireon backup
```

Backs up:

- database;
- investigation records;
- provenance metadata;
- selected evidence;
- configuration excluding secrets unless explicitly requested.

---

# 114. Restore

```text
vireon restore <package>
```

Validate checksums and schema compatibility before modifying active data.

---

# 115. Diagnostics

Single diagnostic screen/command:

```text
OS
CPU
RAM
storage
Tor
browser
AI providers
database
index
network
permissions
```

Provide actionable failures rather than “something went wrong.”

---

# 116. Observability

Metrics include:

- crawl throughput;
- page latency;
- queue depth;
- browser memory;
- CPU utilization;
- memory pressure;
- disk usage;
- index write latency;
- LLM latency;
- model error rate;
- citation validation failures;
- evidence density;
- duplicate rate.

---

# 117. Audit Log

Record:

```text
time
research_id
user_action
source
acquisition_state
result
```

Do not record secrets.

---

# 118. Reproducibility

Every investigation records:

- Vireon version;
- dependency manifest;
- model identifier;
- retrieval configuration;
- source URL;
- retrieval timestamp;
- source hash where available;
- evidence IDs.

Live web research is not perfectly reproducible because external content can change; the system must preserve what it observed.

---

# 119. Release Packaging

Recommended deliverables:

```text
Vireon-INTEL-Windows-x64.exe
Vireon-INTEL-Linux-x86_64.AppImage
Vireon-INTEL-Linux-x86_64.tar.gz
SHA256SUMS
SBOM.json
THIRD_PARTY_MANIFEST.json
LICENSES/
USER_GUIDE.pdf
ADMIN_GUIDE.pdf
```

---

# 120. No-Edit Deployment Requirement

The customer must not need to:

- edit Python files;
- edit YAML source files;
- modify TypeScript;
- install random packages from GitHub;
- manually patch dependencies;
- manually modify Tor configuration for ordinary startup;
- alter source code to set API keys.

User-specific settings belong in UI configuration.

---

# 121. Custom AI Provider Persistence

The AI settings page writes to a versioned configuration store.

The system should support:

```text
Add
Edit
Delete
Test
Enable
Disable
Set priority
Duplicate provider
```

---

# 122. Provider Profiles

A user might have:

```text
Provider A
OpenRouter / strong research model

Provider B
local model / extraction

Provider C
private company endpoint

Provider D
backup model
```

The router selects among them according to task policy.

---

# 123. AI Failover

If the preferred model fails:

```text
Provider A
 ↓
retry
 ↓
Provider B
 ↓
continue research
```

Do not automatically fall back to a model that cannot fit the research context.

---

# 124. Search Result Citation Integrity

Citations are attached to claims rather than simply appended to the bottom of a response.

Internally:

```text
Claim → Evidence → Source
```

A citation validator checks whether the evidence actually supports the claim.

---

# 125. Evidence Strength

Evidence strength may incorporate:

```text
source independence
source quality
passage specificity
freshness
corroboration
contradiction
```

A page containing a rumor does not become a confirmed fact merely because a model can summarize it convincingly.

---

# 126. Analyst-Friendly Language

The UI should distinguish:

```text
Observed
Reported
Inferred
Corroborated
Contradicted
Unverified
Unavailable
```

This prevents overclaiming.

---

# 127. Full Research Trace

Advanced users can inspect:

```text
queries executed
sources discovered
sources rejected
crawl paths
documents extracted
evidence selected
claims formed
contradictions found
sources used in final answer
```

This is one of the features that makes the product feel like professional intelligence software rather than a generic chatbot.

---

# 128. Search Scope

Support:

```text
Onion
Clear web
Local index
Mixed
Specific site
Specific investigation
```

The user may choose the scope when useful, but the engine may also determine that mixed retrieval is necessary.

---

# 129. Domain/Source Constraints

Support:

- include sites;
- exclude sites;
- source types;
- language;
- time ranges;
- specific investigations;
- indexed-only mode;
- live-only mode.

---

# 130. Local Corpus Mode

An analyst can ask:

> “Answer this using only my indexed corpus.”

This is a valuable offline/controlled-research mode.

---

# 131. Live + Local Mode

Default for deep research:

```text
existing local evidence
+
fresh live acquisition
```

This avoids redundant crawling.

---

# 132. Search Freshness Negotiation

The planner determines whether a live refresh is necessary.

Example:

```text
historical event research
→ local index may be sufficient

current event
→ live refresh required

mixed query
→ local + targeted live refresh
```

---

# 133. Service History

For each onion service maintain:

```text
first_seen
last_seen
observed URLs
content changes
availability history
metadata changes
linked services
```

---

# 134. Site Mirroring Detection

Identify services that appear to contain highly overlapping content.

Use:

- content similarity;
- link overlap;
- title structure;
- metadata;
- observed references.

Do not present mirror copies as independent confirmation.

---

# 135. Source Dependency Graph

A research record should allow a user to see:

```text
Claim
 ↓
Source A
 ↓
Source B
```

if A cites or copies B.

This improves intelligence quality substantially.

---

# 136. Report Narrative

The report generator should produce:

```text
Executive answer
Key findings
Evidence summary
Timeline
Entities
Conflicting information
Source ledger
Methodology
Research limitations
```

---

# 137. “Perplexity of the Onion Web” Acceptance Test

Vireon passes its central product test when the user can submit a complex question and the system can:

1. decompose it;
2. search existing indexed knowledge;
3. identify freshness gaps;
4. discover relevant onion services;
5. crawl relevant public/authorized pages;
6. understand forum structure;
7. follow relevant pagination;
8. extract documents and evidence;
9. avoid sending the entire corpus to the LLM;
10. resolve duplicate information;
11. detect conflicting claims;
12. maintain a research graph;
13. generate a detailed synthesis;
14. attach complete source URLs;
15. show the evidence underlying major claims;
16. state what could not be established.

---

# 138. “One Product > 30 Tools” Acceptance Test

There must be no requirement for the operator to open:

- a separate crawler;
- a separate metadata tool;
- a separate forum scraper;
- a separate search tool;
- a separate browser automation tool;
- a separate indexing service;
- a separate graph-analysis tool;
- a separate LLM UI.

They use one Vireon application.

---

# 139. Core Repository Contract

The final source tree should approximately be:

```text
vireon-intel/
├── apps/
│   ├── desktop/
│   └── cli/
├── core/
│   ├── research/
│   ├── scheduler/
│   ├── acquisition/
│   ├── frontier/
│   ├── network/
│   ├── documents/
│   ├── indexing/
│   ├── retrieval/
│   ├── provenance/
│   ├── evidence/
│   ├── graph/
│   └── storage/
├── intelligence/
│   ├── planner/
│   ├── expansion/
│   ├── entities/
│   ├── contradiction/
│   ├── corroboration/
│   ├── temporal/
│   └── synthesis/
├── browser/
│   ├── acquisition/
│   ├── observation/
│   ├── actions/
│   └── recovery/
├── extraction/
│   ├── crawl4ai/
│   ├── html/
│   ├── forums/
│   ├── documents/
│   └── metadata/
├── ai/
│   ├── providers/
│   ├── routing/
│   ├── embeddings/
│   ├── reranking/
│   └── context/
├── database/
│   ├── migrations/
│   └── schema/
├── ui/
├── installers/
│   ├── windows/
│   └── linux/
├── tests/
├── docs/
└── third_party/
```

The exact tree may evolve; the architectural boundaries must not become fragmented into external apps.

---

# 140. Final Product Experience

The user opens Vireon.

They see one research box.

They type a question.

Vireon does the work.

Behind the scenes it may use:

```text
Robin-derived OSINT patterns
Scira-derived research planning
Perplexica/Vane-derived provider/search patterns
Crawl4AI-derived extraction
Browser Use-derived browser reasoning
Cua-derived computer-use control
OnionScan-derived service observation
forum-crawler patterns
onion discovery engines
hybrid search
vector retrieval
graph traversal
entity extraction
historical analysis
```

But the user sees none of these as isolated tools.

They see:

> **Vireon-INTEL**

and a research result.

---

# 141. Strategic Differentiator

The most important feature is not the number of integrations.

It is the **adaptive research loop**:

```text
Question
 ↓
Hypothesis
 ↓
Retrieve
 ↓
Observe
 ↓
Discover
 ↓
Retrieve again
 ↓
Compare
 ↓
Index
 ↓
Reason
 ↓
Verify
 ↓
Answer
```

This turns Vireon from a crawler into a research system.

---

# 142. Final Architecture Statement

Vireon-INTEL is defined as:

> **A local-first, continuously indexed, evidence-grounded, agentic research engine for the onion web that combines adaptive discovery, Tor-routed acquisition, browser rendering, semantic extraction, hybrid retrieval, entity/relationship analysis, temporal versioning, context compilation and LLM synthesis into one cross-platform product.**

Its differentiator is not that it contains many tools.

Its differentiator is that **the research planner can make the entire acquisition and intelligence substrate behave like one coherent machine**.

---

# 143. Release Definition

A release is considered flagship-ready only when all of the following are true:

```text
[ ] clean Windows installation works
[ ] clean Linux installation works
[ ] 5850U/16GB-class portable system remains responsive
[ ] no uncontrolled RAM growth
[ ] crawler is resumable
[ ] browser workers are bounded
[ ] Tor health is observable
[ ] custom AI provider can be configured from UI
[ ] OpenAI-compatible endpoints work
[ ] model selection works
[ ] query planning works
[ ] multi-stage discovery works
[ ] persistent indexing works
[ ] hybrid retrieval works
[ ] forum traversal works
[ ] pagination works
[ ] document extraction works
[ ] duplicate detection works
[ ] entity extraction works
[ ] provenance works
[ ] contradiction detection works
[ ] token-budget compiler works
[ ] citation validation works
[ ] full URLs are preserved
[ ] reports preserve evidence
[ ] investigations survive process crashes
[ ] backups/restores work
[ ] SBOM exists
[ ] licenses are audited
[ ] release dependencies are pinned
```

---

# 144. Non-Negotiable Quality Rules

1. **No fabricated sources.**
2. **No fake progress indicators.**
3. **No hidden truncation of evidence without logging.**
4. **No silent failure converted into “no result.”**
5. **No hard-coded API keys.**
6. **No source-code editing required for ordinary configuration.**
7. **No unbounded browser spawning.**
8. **No sending entire crawls to an LLM.**
9. **No treating duplicate copies as independent corroboration.**
10. **No loss of provenance during summarization/compression.**
11. **No dependence on one model vendor.**
12. **No dependence on a discrete GPU.**
13. **No mandatory Docker environment.**
14. **No silent deletion of evidence due to storage pressure.**
15. **No claims stronger than their evidence.**

---

# 145. Final Positioning

Vireon-INTEL should not be marketed technically as:

> “30 dark-web tools in one app.”

That undersells the architecture.

The product is better described as:

> **The unified research layer over the onion web: a Perplexity-style intelligence engine with its own adaptive crawling, persistent indexing, evidence graph, browser acquisition, temporal knowledge base and citation-grounded synthesis.**

The $5,400-class value is therefore derived from **integration quality, research reliability, persistent knowledge, provenance, recoverability, deployment quality, operator experience and analytical depth**, rather than simply the number of upstream repositories incorporated.

---

# 146. Current Reference Notes

This blueprint was calibrated against the current public project descriptions and repositories where available.

- Robin: AI-powered dark-web OSINT, LLM query refinement, search/result filtering, investigation summaries and multi-provider support. [https://github.com/apurvsinghgautam/robin](https://github.com/apurvsinghgautam/robin)
- Scira: agentic research/search architecture centered on planning, retrieval, cross-checking and cited synthesis. [https://github.com/zaidmukaddam/scira](https://github.com/zaidmukaddam/scira)
- Perplexica/Vane fork: open AI search engine and provider/search abstraction. [https://github.com/BunsDev/perplexica-search-engine-ai](https://github.com/BunsDev/perplexica-search-engine-ai)
- Crawl4AI: asynchronous LLM-friendly crawling, Markdown extraction, deep crawl, structured extraction, caching, screenshots and dynamic-page support. [https://github.com/unclecode/crawl4ai](https://github.com/unclecode/crawl4ai)
- Browser Use: browser-agent framework and browser interaction architecture. [https://github.com/browser-use/browser-use](https://github.com/browser-use/browser-use)
- Cua: cross-OS computer-use drivers and page-aware browser/desktop control. [https://github.com/trycua/cua](https://github.com/trycua/cua)

These projects are **inputs to the architecture, not the final product boundary**.

---

# 147. Final Engineering Principle

The flagship version of Vireon-INTEL should always optimize in this order:

```text
Research correctness
    > evidence integrity
    > reliability
    > sustained performance
    > latency
    > convenience
    > cosmetic complexity
```

For the 5850U/16 GB reference machine, this means choosing **high information gain per CPU cycle and per megabyte**, not raw crawl-volume benchmarks.

For the product as a whole, it means choosing **truthful, inspectable research over impressive-looking answers**.

That is the standard required for Vireon-INTEL to be a genuinely flagship product rather than a polished collection of open-source components.
