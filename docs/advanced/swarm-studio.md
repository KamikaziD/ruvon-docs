# Swarm Studio 3D

Swarm Studio 3D is a live browser demo that runs **64 `RuvonEdgeAgent` instances per tab**, coordinated into a unified 3D drone swarm with no server required.

Open multiple tabs from the same sovereign link and they form a **private fog network**: a BroadcastChannel mesh elects a leader, divides the formation space, and each browser renders its own slice of the fleet.

!!! tip "Try it live"
    **[Launch the demo →](https://kamikazid.github.io/ruvon-swarm/)**
    · [How it works](https://kamikazid.github.io/ruvon-swarm/info.html)
    · [GitHub](https://github.com/KamikaziD/ruvon-swarm)

---

## Architecture

```
BROWSER TAB (sovereign)                     BROWSER TAB (follower N)
┌───────────────────────────────┐           ┌───────────────────────────────┐
│  RuvonEdgeAgent (Pyodide)     │           │  RuvonEdgeAgent (Pyodide)     │
│  DroneCommand workflow        │           │  DroneCommand workflow        │
│  SwarmFormation workflow      │           │  SwarmFormation workflow      │
├───────────────────────────────┤           ├───────────────────────────────┤
│  mesh_brain.js (Worker)       │◄─────────►│  mesh_brain.js (Worker)       │
│  BroadcastChannel gossip      │           │  gossip + follower election   │
│  Sovereign election           │           │  FORMATION_INTENT receiver    │
├───────────────────────────────┤           ├───────────────────────────────┤
│  physics_worker.js × 4 (SAB) │           │  physics_worker.js × 4 (SAB) │
│  Reynolds flocking + PID alt  │           │  Reynolds flocking + PID alt  │
├───────────────────────────────┤           ├───────────────────────────────┤
│  renderer_webgpu.js           │           │  renderer_webgpu.js           │
│  Instanced drone mesh         │           │  Ghost dots for remote squads │
└───────────────────────────────┘           └───────────────────────────────┘
              │   BroadcastChannel (same-device)
              └──────────────────────────────────►
```

Each tab runs completely independently. Computation stays on the device — no round-trips, no shared backend.

---

## Quick start

### Run locally

```bash
git clone https://github.com/KamikaziD/ruvon-swarm
cd ruvon-swarm/examples/swarm_studio
python serve.py          # http://localhost:8081
```

`serve.py` adds the COOP/COEP headers required for `SharedArrayBuffer` and proxies the Pyodide CDN through localhost (cached after first download).

### Add a second tab

1. Copy the **Sovereign Link** from the top-left panel.
2. Paste it into a new tab — the follower joins the mesh.
3. Click **Join Formation** on the follower. The fleet doubles to 128 drones.
4. Repeat for more tabs.

---

## ruvon-swarm package

Install the Python workflow package independently:

```bash
pip install ruvon-swarm
```

### Workflows

| Workflow | Steps | Purpose |
|----------|-------|---------|
| `DroneCommand` | 5 | Parse → validate → build intent → log → execute |
| `SwarmFormation` | 5 | Same pipeline, intent-first output |
| `SwarmHealth` | 2 | Telemetry recording + battery health check |

### State models

```python
from ruvon_swarm.state_models import SwarmFormationState, DroneCommandState

state = SwarmFormationState(
    command="form a sphere with 128 drones",
    global_count=128,
)
```

### Step functions

```python
from ruvon_swarm.steps.formation import (
    parse_command,
    validate_formation,
    build_intent,
    log_formation,
    execute_formation,
)
```

Each step follows the standard ruvon signature:

```python
def parse_command(state, context, **kwargs) -> dict:
    # Returns dict merged into workflow state
    ...
```

### Deterministic PRNG

`build_intent` returns a seed derived from the workflow ULID. The seed drives `mulberry32` — a PRNG with bitwise-identical implementations in both JavaScript and Python — so formation math can be validated server-side without re-running the renderer.

```python
from ruvon_swarm.utils import mulberry32

rng = mulberry32(seed=0xCAFEBABE)
x = rng()   # float in [0, 1)
```

---

## Testing

```bash
cd ruvon-swarm
pip install -e . "ruvon-sdk>=0.1.2" pytest pytest-asyncio
pytest tests/ -v
# 27 passed
```

Tests use `TestHarness` (in-memory providers, no database required):

```python
from ruvon.testing import TestHarness

async def test_build_intent_sphere():
    h = TestHarness(workflow_type="SwarmFormation")
    result = await h.run_step(
        "build_intent",
        {"preset": "sphere", "global_count": 128},
    )
    assert result["intent"]["preset"] == "sphere"
    assert result["intent"]["seed"] != 0
```

---

## Sovereignty election

Every tab gossips a scored **capability vector** (battery, CPU load, RAM, task queue) every ~2 s. The highest-scoring pod becomes sovereign.

| Mechanism | Purpose |
|-----------|---------|
| `FOUNDING_BIAS = 0.40` | First tab opened stays sovereign while alive |
| Stable `pod_id` tiebreaker | All tabs converge to the same winner at equal scores |
| `electLeaderFresh()` | Resolves split-brain without hysteresis when two pods simultaneously claim sovereignty |
| Main-thread GOODBYE on `pagehide` | Peers re-elect in <1 s instead of waiting 15 s stale timeout |

When sovereignty transfers, the new sovereign:

1. Resets its slot to 0 (sovereign is always slot 0).
2. Adopts all currently flying pods as active fleet members.
3. Rebroadcasts the current formation with the corrected slot map.

---

## Browser requirements

| Requirement | Reason |
|-------------|--------|
| Chrome ≥ 113 or Edge ≥ 113 | WebGPU |
| COOP + COEP headers | `SharedArrayBuffer` for SAB physics |
| `localStorage` | IndexedDB telemetry (optional) |

Firefox support is pending WebGPU shipping in stable (expected 2025).

When deployed on GitHub Pages, `coi-serviceworker.js` intercepts all responses and injects the COOP/COEP headers automatically — no server configuration required.
