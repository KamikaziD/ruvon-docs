# Changelog

All notable changes to Ruvon SDK are documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and the project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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
