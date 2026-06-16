# Alethic-ISM Architecture

> **Status: work in progress.** This document is incomplete and under active revision — some sections may be out of date or pending detail.

---

## System Overview

```
  +-------------------------------------------------------------------------+
  |                          ALETHIC STUDIO (UI)                            |
  |          Visual graph editor, monitor, debugger, AI assistant           |
  +-------------------------------------------------------------------------+
                                    |
  +-------------------------------------------------------------------------+
  |                            API LAYER                                    |
  |                                                                         |
  |  Core API        Stream API       Query API        Vault API            |
  |  NLP API         Usage API        Embeddings API   Logger API           |
  |  ...and others (multiple versions, independently deployable)            |
  +-------------------------------------------------------------------------+
                                    |
  +-------------------------------------------------------------------------+
  |                        EXECUTION ENGINE                                 |
  |        Dependency resolution, node evaluation, state routing            |
  +-------------------------------------------------------------------------+
         |                         |                         |
  +------------------+   +---------------------+   +-------------------+
  |    PROCESSORS    |   |   STATE ROUTING     |   |    PERSISTENCE    |
  |    (pluggable)   |   |   (pluggable)       |   |    (pluggable)    |
  |                  |   |                     |   |                   |
  | OpenAI / Claude  |   | NATS                |   | PostgreSQL        |
  | Gemini / Llama   |   | Kafka               |   | S3 / Block store  |
  | Python / Lua     |   | Pub-sub             |   | DFS               |
  | MCP / A2A        |   | Cross-cluster       |   | Per-state config  |
  | Template         |   | Dynamic workload    |   | Tiered storage    |
  | API / Webhook    |   | Multiple versions   |   | Multiple versions |
  +------------------+   +---------------------+   +-------------------+
```

---

## Core Model

```
  State (v1)                          State (v2)
  immutable                           immutable
      |                                   |
      +--[ edge function ]-----------> [ Node: Instruction + Processor ] ---+--[ edge function ]-->
```

**Nodes** pair an instruction with a processor. Instruction = *what* (LLM prompt, code, API call, MCP, A2A, template, query). Processor = *how*. Contract: immutable state in, immutable state out.

**Edges** are programmable functions on inputs and outputs — drop, pass, retry, transform, or branch. Inputs can repeat. Concurrency is configurable per-edge via expressions on data. Debug triggers fire on matching data expressions.

**States** are immutable, versioned, and carry full lineage. Types: computation, interactive (HITL), memory (RAG), data source (S3, files, images, Excel). All states emit via streaming — it's built into the propagation layer. Cross-state queries reference data across states.

---

## Execution Flow

```
  API request --> Message broker --> State Router --> Processor --> State Propagation
                                        |                              |
                                   Batch loading                  Persist + Route
                                   Status: ROUTED                 to downstream
                                                                       |
                                                                  Edge functions
                                                                  evaluate on output
```

**Lifecycle**: `CREATED -> ROUTE -> ROUTED -> QUEUED -> RUNNING -> COMPLETED | FAILED`

**Batching**: Two levels — database batch size (rows per query) and output batch size (rows per message). Both configurable per processor.

**Propagation**: Chainable providers — in-memory update, persist to store, forward to downstream processors.

---

## Processors

| Processor | Capability |
|-----------|-----------|
| OpenAI | GPT, DALL-E |
| Anthropic | Claude |
| Gemini | Google Gemini |
| OpenRouter | Unified multi-model proxy |
| Llama | Local/self-hosted models |
| Python / Lua | Sandboxed code execution |
| Template | Mako rendering |
| MCP / A2A | Protocol integrations |
| API / Webhook | External service calls |
| Cross-Join | Cartesian product of states |
| Join | Windowed online inner join (log2 timescale) |
| Merge | Composite output on shared keys |
| Tables | Batched DB operations |
| File Source | S3/file ingest (CSV, Excel, Parquet, PDF, images) |
| Memory | RAG, embeddings, context retrieval |
| Data Source | External database queries |

---

## Routing (Pluggable)

Abstracted via `BaseRoute` interface: `connect`, `publish`, `subscribe`, `consume`, `flush`.

| Backend | Status |
|---------|--------|
| NATS | Production |
| Kafka | Planned |
| Custom | Implement interface |

Two routing modes: `query_state_entry` (state-to-processor) and `query_processor_entry` (direct-to-processor). Multiple versions — V2 supports cross-cluster routing.

---

## Persistence (Pluggable)

| Backend | Use Case |
|---------|----------|
| PostgreSQL | Primary storage |
| S3 | Large data, archival |
| Block store | High-performance (LRU cache, compaction) |
| DFS | Distributed, high availability |
| Custom | Implement interfaces |

**Per-state configuration**: each state can specify its own storage backend. Metadata in PostgreSQL, large data in S3, hot data in block store — all transparent to the graph.

---

## APIs

| API | Purpose |
|-----|---------|
| Core | State/processor/template/route CRUD, execution |
| Stream | WebSocket + pub-sub bidirectional streaming |
| Query | ISM-QL, low-latency state retrieval |
| Embeddings | Vector search (pgvector) |
| NLP | Embedding generation, semantic search |
| Vault | Secrets (AES-256-GCM), per-tenant/user/project |
| Usage | Token/cost metrics |
| Logger | Instruction-level debugging |

Auth: JWT (Firebase or local). Multi-tenant — all entities scoped to project.

---

## Scaling

| Component | Strategy |
|-----------|----------|
| APIs | Replicas behind load balancer |
| State Router | Multiple consumers on same topic |
| Processors | Multiple workers per type |
| Database | Read replicas, connection pooling |
| Message broker | Clustered deployment |

---

## Security

| Area | Approach |
|------|----------|
| Auth | JWT (Firebase / local) |
| Multi-tenancy | Project-scoped entities |
| Secrets | Vault API, AES-256-GCM, per-tenant/user/project/processor |
| Authorization | Project-level access checks |

---

## Training Loop

```
  Graph (expensive)                    Model (fast)

  In --> [A] --> [B] --> [C] --> Out   In --> [Model-v1] --> Out
               |                                  ^
               +---> training data --------> fine-tune
```

1. Execute graph, collect provenanced outputs
2. Export as training pairs
3. Fine-tune model
4. Register as processor
5. Plug into new graph or compare via cross-join
6. Iterate

**Examples:**

```
Normative reasoning:
  Before: Scenario -> Claude -> GPT-4 -> Consensus -> Rules -> Decision
                                | training data
  After:  Scenario -> Ethics-Reasoner-v1 (trained model) -> Decision

Contingent valuation:
  Before: Demographics x Beliefs x Bids -> [Multiple LLMs] -> Vote -> Logit -> WTP estimate
                                            | training data
  After:  Demographics x Beliefs x Bids -> WTP-Estimator-v1 -> WTP estimate
```

The trained model encapsulates the multi-step reasoning in a single inference call.

---

## Extensibility

**Custom processor**: implement interface, subscribe to topic, register as `ProcessorProvider` — appears in Studio.

**Custom router**: implement `BaseRoute`, add to routing config.

**Custom storage**: implement storage interfaces, inject at init. Composite storage routes per-state.

---

## Configuration

| Variable | Purpose |
|----------|---------|
| `DATABASE_URL` | Database connection |
| `ROUTING_FILE` | Routing YAML |
| `NATS_URL` | Broker address |
| `STATE_BATCH_SIZE` | Rows per batch |
| `ENABLED_LOCAL_AUTH` | Local auth toggle |
| `ENABLED_FIREBASE_AUTH` | Firebase auth toggle |

```yaml
# .routing.yaml
routes:
  - selector: processor/state/router
    type: nats
    subject: processor.state.router
  - selector: processor.models.openai
    type: nats
    subject: processor.models.openai
  - selector: processor/state/sync
    type: nats
    subject: processor.state.sync
```

---

## Outputs

Each graph run produces:

- Final and intermediate states (all versioned)
- Instruction-level metadata (type, processor, duration, dependencies)
- Logs of model completions or function returns
- Full execution trace
- Optional exports: JSON summaries, CSV tables, Excel, serialized replay data
- Publishable, immutable result snapshots (graph + dashboards + data)
- Training data pairs for model fine-tuning
