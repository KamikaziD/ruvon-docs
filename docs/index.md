# Ruvon SDK

**Python-native workflow engine for fintech edge devices and cloud.**

Ruvon lets you define workflows once in YAML and run them on POS terminals, ATMs, mobile readers, cloud workers, and in the browser — with full offline resilience.

<div class="grid cards" markdown>

-   **:material-rocket-launch: Getting Started**

    ---

    Install Ruvon and run your first workflow in five minutes.

    [:octicons-arrow-right-24: Tutorial](tutorials/getting-started.md)

-   **:material-map: How-To Guides**

    ---

    Step-by-step guides for common tasks.

    [:octicons-arrow-right-24: Guides](how-to-guides/installation.md)

-   **:material-book-open: Explanation**

    ---

    Understand the architecture and design decisions.

    [:octicons-arrow-right-24: Architecture](explanation/architecture.md)

-   **:material-api: Reference**

    ---

    Complete API and configuration reference.

    [:octicons-arrow-right-24: API Docs](reference/api/workflow.md)

</div>

## Ecosystem

| Package | Description | Install |
|---------|-------------|---------|
| [`ruvon-sdk`](https://pypi.org/project/ruvon-sdk/) | Core SDK + CLI | `pip install ruvon-sdk` |
| [`ruvon-edge`](https://pypi.org/project/ruvon-edge/) | Edge agent (SQLite, SAF, WASM) | `pip install ruvon-edge` |
| [`ruvon-server`](https://pypi.org/project/ruvon-server/) | Cloud control plane (FastAPI) | `pip install ruvon-server` |

## Docker

```bash
docker pull ruvondev/ruvon-server:0.1.1
docker pull ruvondev/ruvon-worker:0.1.1
docker pull ruvondev/ruvon-dashboard:0.1.1
docker pull ruvondev/ruvon-edge-dev:0.1.1
```

## Quick Example

```python
from ruvon.builder import WorkflowBuilder
from ruvon.implementations.persistence.sqlite import SQLitePersistenceProvider
from ruvon.implementations.execution.sync import SyncExecutor

builder = WorkflowBuilder("config/workflow_registry.yaml")
workflow = builder.create_workflow(
    "LoanApplication",
    persistence_provider=SQLitePersistenceProvider(":memory:"),
    execution_provider=SyncExecutor(),
)
```

## Links

- [GitHub: ruvon-sdk](https://github.com/KamikaziD/ruvon-sdk)
- [GitHub: ruvon-docs](https://github.com/KamikaziD/ruvon-docs)
- [Docker Hub: ruvondev](https://hub.docker.com/u/ruvondev)
