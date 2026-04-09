# Roadmap

Planned features and priorities for the Ruvon SDK.

**Current stable version:** 0.1.1  
**Last updated:** 2026-04-07

---

## Near Term (0.2.x)

- **GraphQL step type** — Query and mutate GraphQL endpoints from workflow YAML
- **Loop step enhancements** — Break/continue conditions, nested loop support
- **Cron scheduler improvements** — Better timezone handling, conflict detection
- **Observability** — Built-in Prometheus metrics endpoint for workflow throughput and latency
- **Workflow tags and search** — Tag workflows at creation time; filter by tag in `ruvon list`

---

## Medium Term (0.3.x — 0.4.x)

- **Workflow composition UI** — Visual YAML builder in the dashboard
- **Multi-tenant fleet** — Organization-level device and workflow isolation
- **gRPC step type** — Call gRPC services from workflow steps
- **Distributed tracing** — OpenTelemetry integration (spans per step, trace propagation)
- **Workflow templates** — Shareable, parameterized workflow blueprints

---

## Longer Term (1.0.0)

**1.0.0 represents a stable API guarantee** — no breaking changes to the provider interfaces, YAML schema, or CLI commands after this point.

Planned additions before 1.0.0:

- **Debug UI** — Rich web interface for step-by-step workflow inspection
- **Event-driven triggers** — Start workflows from Kafka, SQS, Pub/Sub events
- **Native ARM WASM** — Pre-built WASM binaries for `linux/arm64` edge hardware
- **Federated edge sync** — Edge-to-edge workflow state synchronisation without cloud

---

## Feature Requests

Open a GitHub issue: [github.com/KamikaziD/ruvon-sdk/issues](https://github.com/KamikaziD/ruvon-sdk/issues)
