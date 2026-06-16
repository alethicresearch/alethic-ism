# Modules

The Alethic ISM system spans many independently versioned and deployable components. This is a reference list; see the [README](../README.md) for the overview.

---

> Alethic-ISM is a multi-language system (Python, Go, Rust, TypeScript) spread across many independently versioned and deployable modules. The open-source baseline below tracks a stable subset; several newer capabilities — the Go state-storage tier, the publishing/viewer surface, and the enterprise Studio — currently run ahead of the public baseline and are marked **Enterprise**. We upstream commercial features over time. For research, academic, or commercial use, [reach out](#contact) — access and collaboration terms are flexible.

### Core Libraries

- **[alethic-ism-core](https://github.com/quantumwake/alethic-ism-core.git) (Python SDK):**
  Core state machine logic, storage interfaces, and processor base classes. Defines the abstract interfaces that make persistence and routing pluggable.

- **[alethic-ism-db](https://github.com/quantumwake/alethic-ism-db.git) (Python SDK):**
  Implements the storage interfaces for PostgreSQL. Other backends follow the same interface contracts.

- **[alethic-ism-core-go](https://github.com/quantumwake/alethic-ism-core-go.git) (Go SDK):**
  The Go backbone for state and processor management — repositories for state, processors, vault, usage, and traces; generic NATS routing, S3 streaming, caching, encryption (AES-GCM), and JWT/Gin auth middleware. Heavily expanded; underpins all Go-centric services.

- **[alethic-ism-core-rust](https://github.com/quantumwake/alethic-ism-core-rust.git) (Rust SDK):**
  Core and state functionality for Rust-centric, latency-critical components. *Available on request.*

### State Storage

The storage layer is pluggable through interfaces; states can be persisted per-state to different backends. The default high-throughput tier is **StateFS**, a columnar, tiered, content-addressed store.

- **statefs (Go engine):**
  Embeddable storage engine — append-only Parquet blocks, a SQLite manifest, tiered local + S3 (block) storage, background compaction, and snapshotting. State rows are immutable and queryable. *Enterprise.*

- **[alethic-ism-statefs-node](https://github.com/quantumwake/alethic-ism-statefs-node.git) (Go):**
  Runnable service around the StateFS engine: consumes state from NATS, exposes an HTTP API (read/write/schema/profile/compact), runs an in-process SQL query engine (DuckDB) over the blocks, and produces immutable snapshots for publishing. *Enterprise.*

- **[alethic-ism-state-sync-store](https://github.com/quantumwake/alethic-ism-state-sync-store.git) (Python):**
  State-sync persistence and forwarding based on configured routing rules. The original sync tier, alongside the newer StateFS path.

- **[alethic-ism-state-tables](https://github.com/quantumwake/alethic-ism-state-tables.git) (Go):**
  Batched database table operations for efficient bulk state persistence and retrieval.

### API Services

- **[alethic-ism-api](https://github.com/quantumwake/alethic-ism-api.git) (Python):**
  Primary control-plane API for managing states, processors, templates, routes, projects, and execution.

- **[alethic-ism-query-api](https://github.com/quantumwake/alethic-ism-query-api.git) (Go):**
  Rapid retrieval of state data using ISM-QL. Low-latency queries, scalable data access, vault operations, and embedding-based search.

- **[alethic-ism-stream-api](https://github.com/quantumwake/alethic-ism-stream-api.git) (Go):**
  Boundary proxying and bidirectional streaming of state data. Supports consumer subscriptions to the ISM network and cluster-wide state routing.

- **[alethic-ism-nlp-api](https://github.com/quantumwake/alethic-ism-nlp-api.git) (Python):**
  NLP services — text embeddings (OpenAI, SentenceTransformers) with pgvector similarity search, and an assistant chat endpoint (OpenAI / Anthropic) with tool-calling and RAG context injection. Powers Studio's AI assistant and semantic search.

- **[alethic-ism-vault-api](https://github.com/quantumwake/alethic-ism-vault-api.git) (Go):**
  Manages secrets and tokens (AES-256-GCM) for tenants, users, teams, projects, and individual processor steps.

- **[alethic-ism-usage](https://github.com/quantumwake/alethic-ism-usage.git) (Go):**
  Persists usage data for any state processor and exposes a REST API for querying usage metrics.

- **[alethic-ism-dashboard-api](https://github.com/quantumwake/alethic-ism-dashboard-api.git) (Go):**
  Stores, retrieves, and shares analytics dashboard configurations scoped by project; dashboards embed into published snapshots.

- **[alethic-ism-publish-api](https://github.com/quantumwake/alethic-ism-publish-api.git) (Go):**
  Freezes a project into an immutable, read-only snapshot on object storage and serves it via unguessable capability-based share links. The backend for [published results](#published-results). *Enterprise.*

- **[alethic-ism-monitor](https://github.com/quantumwake/alethic-ism-monitor.git) (Python):**
  State-transition reporting and logging. A v2 rewrite in Go is planned.

### Instruction Processors

- **[alethic-ism-processor-openrouter](https://github.com/quantumwake/alethic-ism-processor-openrouter.git) (Python):**
  Executes instructions via OpenRouter as a unified proxy to many AI models through a single interface.

- **[alethic-ism-processor-openai](https://github.com/quantumwake/alethic-ism-processor-openai.git) (Python):**
  Executes instructions using OpenAI models, including GPT and image generation.

- **[alethic-ism-processor-anthropic](https://github.com/quantumwake/alethic-ism-processor-anthropic.git) (Python):**
  Executes instructions using Anthropic Claude models.

- **[alethic-ism-processor-gemini](https://github.com/quantumwake/alethic-ism-processor-gemini.git) (Python):**
  Executes instructions using Google Gemini models.

- **[alethic-ism-processor-python](https://github.com/quantumwake/alethic-ism-processor-python.git) (Python):**
  Executes sandboxed Python code (via RestrictedPython) against a state input to produce the output state.

- **[alethic-ism-processor-mako](https://github.com/quantumwake/alethic-ism-processor-mako.git) (Python):**
  Renders Mako templates against a state input for structured data transformation.

- **[alethic-ism-processor-llama](https://github.com/quantumwake/alethic-ism-processor-llama.git) (Go):**
  Executes instructions using Llama-compatible model APIs for local or self-hosted inference.

- **[alethic-ism-edge-function](https://github.com/quantumwake/alethic-ism-edge-function.git):**
  Programmable per-edge functions (Lua) that validate, calibrate, filter, branch, or transform state as it flows between nodes — catching bad output before it propagates.

### Data Ingestion & Sources

- **[alethic-ism-file-source](https://github.com/quantumwake/alethic-ism-file-source.git) (Go):**
  Ingests files from object storage into the graph — CSV/TSV/XLSX/Parquet into structured rows, and PDF (with OCR) into text chunks — resolving credentials via the vault and emitting state over NATS.

- **[alethic-ism-ds](https://github.com/quantumwake/alethic-ism-ds.git) (Go):**
  Connects to external data sources (e.g., SQL databases) as data-source state instructions. *Available on request.*

- **[alethic-ism-memory](https://github.com/quantumwake/alethic-ism-memory.git) (Go):**
  Memory processor for LLMs — stores and retrieves context for RAG and context-aware processing, with embedding-based retrieval via pgvector. *Available on request.*

### State Transformers

These processors merge or compose multiple inputs into combined output states:

- **[alethic-ism-state-online-cross-join](https://github.com/quantumwake/alethic-ism-state-online-cross-join.git) (Python):**
  Distributed cartesian product of two states. The foundation for systematic evaluation — run the same prompts across multiple models and parameter sets.

- **[alethic-ism-state-online-join](https://github.com/quantumwake/alethic-ism-state-online-join.git) (Go):**
  Windowed online inner join between two or more states using a log2 timescale, given configured join keys and arrival windows.

### Routing & Persistence

The routing and persistence layers are abstracted through interfaces. Implement `BaseRoute` for custom message brokers, or the storage interfaces for custom backends.

- **[alethic-ism-state-router](https://github.com/quantumwake/alethic-ism-state-router.git) (V1 Python):**
  Dynamically discovers states and routes them to the appropriate processing nodes within the execution graph.

- **alethic-ism-router (V2 Go):**
  Upgraded state router with cross-cluster routing. *Available on request.*

### Web Applications & UI

- **[alethic-ism-ui](https://github.com/quantumwake/alethic-ism-ui.git) (React / TypeScript):**
  Alethic Studio — the open visual workbench for designing, executing, monitoring, and analyzing instruction graphs, with an AI assistant that can build entire pipelines through natural language.

- **alethic-ism-ui-enterprise (React / TypeScript):**
  The actively developed Studio — graph editor, dashboard/chart builder, project management, data exploration (in-browser SQL), and the AI assistant. *Enterprise.*

- **alethic-ism-publish-ui (React / TypeScript):**
  Read-only viewer for [published results](#published-results). Renders the workflow graph, dashboards, charts, and data entirely client-side using an in-browser query engine over the snapshot's columnar data. *Enterprise.*

### Published Component Libraries (npm)

- **[@quantumwake/kgraph](https://github.com/quantumwake/kgraph) (npm):**
  Standalone canvas-based graph rendering engine extracted from Alethic Studio. Pure React, zero external dependencies.

- **[@quantumwake/react-assistant](https://github.com/quantumwake/react-assistant) (npm):**
  Generic AI assistant engine with a context-provider pattern, extracted from Alethic Studio.

- **[@quantumwake/terminal-ux-components](https://github.com/quantumwake/terminal-ux-components) (npm):**
  Terminal-style design-system primitives (inputs, dialogs, menus, tabs) with a theme-provider contract, shared across the Studio and the viewer.

- **[@quantumwake/terminal-ux-dashboard-components](https://github.com/quantumwake/terminal-ux-dashboard-components) (npm):**
  Dashboard and chart-builder components (Nivo charts, SQL console, draggable grid) using a capability-injection pattern — one component tree, full-CRUD in Studio or read-only in the viewer.

### Experimental & Emerging

- **Alethic ISM Autoscaler:** dynamically provisions cloud compute based on processing demand in multi-tenant environments.
- **Alethic ISM Interactive Action Hooks + UI:** real-time user feedback loops and reinforcement learning during state executions.
- **Alethic ISM Training Studio:** training and fine-tuning models from state data, including automated fine-tuning defined by instruction graphs.
- **Alethic ISM Marketplace:** sharing and discovering processors, workflows, and modules.
- **Alethic ISM MCP Server:** integration with the Model Context Protocol (MCP).
- **Alethic ISM DFS (Rust):** distributed file system exploration for state data with high availability and fault tolerance.
