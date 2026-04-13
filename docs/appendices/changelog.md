# Changelog

All notable changes to Ruvon SDK are documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and the project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [0.1.2] — 2026-04-13

### Fixed

- **SQLite NOT NULL constraint** — Edge SQLite schema now correctly handles nullable columns; `process_saf_sync()` no longer raises `IntegrityError` on first sync after a cold start
- **NATSBridge silent disable** — Server image now installs `nats-py` via the `[nats]` extra in `ruvon-server`; bridge no longer silently disabled due to missing import
- **NATS stream bootstrap** — `ruvon-server` NATSBridge now provisions all 7 JetStream streams at startup (including `RUVON_NODE_PATCH` and `RUVON_MESH_BUILD`); no external bootstrap service required
- **Dashboard realm config** — Fixed hardcoded `realms/rufus` → `realms/ruvon` in dashboard auth; login no longer fails with "Realm does not exist"
- **Dashboard API URL env var** — Fixed `NEXT_PUBLIC_RUFUS_API_URL` → `NEXT_PUBLIC_RUVON_API_URL`; all API calls now use the correct base URL
- **Dashboard login text** — Fixed `"RUFUS EDGE"` → `"Ruvon Edge"` in login page

### Added

- **`relay` extra for `ruvon-edge`** — `pip install 'ruvon-edge[relay]'` installs FastAPI + uvicorn for peer relay / mesh networking HTTP server (required when `PEER_LISTEN_PORT > 0`)

### Changed

- **Docker images** — `ruvondev/ruvon-{server,worker,flower,dashboard,edge-dev}:0.1.2` (multi-arch: `linux/amd64`, `linux/arm64`)

---

## [0.1.1] — 2026-04-07

Initial public release of the **Ruvon SDK** under the `ruvon-*` package names.

### Added

- **Core workflow engine** (`ruvon-sdk`) — YAML-defined workflows with Pydantic state models, provider pattern dependency injection, and all step types: `STANDARD`, `ASYNC`, `DECISION`, `PARALLEL`, `HTTP`, `LOOP`, `FIRE_AND_FORGET`, `CRON_SCHEDULE`, `HUMAN_IN_LOOP`, `AI_INFERENCE`, `WASM`
- **Edge agent** (`ruvon-edge`) — Offline-first runtime for POS terminals, ATMs, and kiosks; SQLite persistence, Store-and-Forward (SAF) queue, ETag-based config push, WASM fraud scorer sidecar
- **Cloud control plane** (`ruvon-server`) — FastAPI REST API for device fleet management, Celery distributed execution, dashboard with real-time workflow monitoring
- **`RuvonEdgeAgent`** — Primary API for edge device deployment with WebSocket command channel and `register_command_handler()` support
- **Paged Inference Runtime** — Shard-level LLM inference for browser (Pyodide/WebGPU) and edge (llama-cli `--mmap`) targets
- **Browser demo** — Self-contained static demo running three workflows (parallel fan-out, IoT anomaly detection, WebGPU risk scoring) entirely in-browser via Pyodide
- **CLI** (`ruvon` command) — Workflow management, database migrations, zombie detection, config management
- **Docker images** — `ruvondev/ruvon-{server,worker,flower,dashboard,edge-dev}:0.1.1` (multi-arch: `linux/amd64`, `linux/arm64`)

### Package names

| PyPI package | Contents |
|---|---|
| `ruvon-sdk` | Core engine + CLI |
| `ruvon-edge` | Edge agent (`ruvon_edge`) |
| `ruvon-server` | Cloud control plane (`ruvon_server`) |

---

## Links

- [PyPI: ruvon-sdk](https://pypi.org/project/ruvon-sdk/)
- [PyPI: ruvon-edge](https://pypi.org/project/ruvon-edge/)
- [PyPI: ruvon-server](https://pypi.org/project/ruvon-server/)
- [GitHub: ruvon-sdk](https://github.com/KamikaziD/ruvon-sdk)
- [Docker Hub: ruvondev](https://hub.docker.com/u/ruvondev)
