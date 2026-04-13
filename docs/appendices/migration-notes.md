# Migration Notes

Version-to-version migration guides for upgrading Ruvon SDK.

---

## Breaking Changes Policy

Before **1.0.0**: minor versions may introduce breaking changes to provider interfaces, YAML schema, or CLI flags. Each breaking change is documented here.

After **1.0.0**: the provider interfaces, YAML step schema, and `ruvon` CLI commands are stable. Only major version bumps introduce breaking changes.

---

## Upgrading to 0.1.2

**From:** 0.1.1

### Changes required

No breaking changes. Drop-in upgrade.

```bash
pip install --upgrade ruvon-sdk==0.1.2 ruvon-edge==0.1.2 ruvon-server==0.1.2
```

### Docker images

```bash
docker pull ruvondev/ruvon-server:0.1.2
docker pull ruvondev/ruvon-worker:0.1.2
docker pull ruvondev/ruvon-dashboard:0.1.2
docker pull ruvondev/ruvon-edge-dev:0.1.2
```

Update your compose file image tags from `:0.1.1` → `:0.1.2`.

### NATS users

If you run the full NATS stack, `nats-init` bootstrap service is no longer needed — `ruvon-server` provisions all 7 JetStream streams at startup. Remove it from your compose file.

---

## Upgrading to 0.1.1

**From:** First public release — no prior version to migrate from.

If you are migrating from a custom workflow engine, see the sections below.

### Import paths

```python
# Core SDK
from ruvon.builder import WorkflowBuilder
from ruvon.workflow import Workflow
from ruvon.models import StepContext, WorkflowPauseDirective

# Persistence providers
from ruvon.implementations.persistence.sqlite import SQLitePersistenceProvider
from ruvon.implementations.persistence.postgres import PostgresPersistenceProvider

# Execution providers
from ruvon.implementations.execution.sync import SyncExecutionProvider
from ruvon.implementations.execution.celery import CeleryExecutionProvider
from ruvon.implementations.execution.thread_pool import ThreadPoolExecutionProvider

# Edge agent
from ruvon_edge.agent import RuvonEdgeAgent
```

### Database migrations

For PostgreSQL, apply Alembic migrations on first deploy:

```bash
cd src/ruvon
export DATABASE_URL="postgresql://user:pass@host:5432/db"
alembic upgrade head
```

SQLite schemas are created automatically on first connection (`SQLitePersistenceProvider(db_path=...)`).

### Docker images

Pull from Docker Hub:

```bash
docker pull ruvondev/ruvon-server:0.1.1
docker pull ruvondev/ruvon-worker:0.1.1
docker pull ruvondev/ruvon-dashboard:0.1.1
docker pull ruvondev/ruvon-edge-dev:0.1.1
```

---

## Future Migrations

Migration notes for 0.2.x and beyond will be added here as breaking changes are introduced.
