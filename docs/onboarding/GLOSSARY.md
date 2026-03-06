# Glossary

Plain-language definitions for every technical term used in this system.

If you encounter an unfamiliar word in the docs, scripts, or workflows, look it up here first.

---

## A

### Action
A reusable unit of work in GitHub Actions. Think of it as a named function you can call from any workflow. This system organizes actions into three layers: [Cadence](#cadence), [Survival](#survival), and [Observability](#observability).

**Plain English:** A building block you snap into a workflow, like a Lego brick.

---

### Adapter
A thin script that wraps [core scripts](#core-scripts) to work in a specific [substrate](#substrate). Adapters translate environment-specific details (paths, variables, credentials) so the same core logic runs everywhere.

**Files:** `scripts/adapters/ci-adapter.sh`, `docker-adapter.sh`, `local-adapter.sh`

**Plain English:** A plug adapter. The appliance (core script) is the same; the adapter makes it fit the local socket.

---

### Artifact
A file or directory produced by a workflow run and uploaded for later retrieval. Metrics, reports, and summaries are stored as artifacts.

**Plain English:** A saved output file from a workflow — like a receipt you keep after a transaction.

---

## B

### Behavior Layer
The middle layer of the [three-layer architecture](#three-layer-architecture). It defines the [cadence](#cadence) — the consistent phases every workflow follows, regardless of what the workflow is doing.

**Actions:** `phase-init`, `phase-prepare`, `phase-execute`, `phase-report`, `phase-cleanup`

**Plain English:** The set of rules that says "every workflow does these steps in this order."

---

## C

### Cadence
The rhythm or sequence that every workflow follows: Init → Prepare → Execute → Report → Cleanup → Seal. The word "cadence" comes from music — it means a regular, recurring beat.

**Plain English:** The standard order of steps every workflow uses, like a checklist that never changes.

---

### Cache
Stored dependency data (libraries, packages, build outputs) that is reused across workflow runs to avoid re-downloading or re-building the same things. A "cache hit" means the stored data was found and used; a "cache miss" means it had to be rebuilt.

**Plain English:** A shortcut — saving your homework so you don't redo it next time.

---

### CI/CD
**Continuous Integration / Continuous Deployment.** CI means automatically running tests every time code changes. CD means automatically deploying code after tests pass.

**Plain English:** Automated testing and shipping of code changes.

---

### Coherence
The state of a system where all parts remain consistent and logical, even under stress. The repository name "operational coherence" refers to keeping workflows coherent (not chaotic) as the system evolves.

**Plain English:** Everything still making sense and fitting together — the opposite of chaos.

---

### Composite Action
A GitHub Action made entirely of other actions and shell steps, without requiring a separate runtime. All actions in this repository are composite actions.

**Plain English:** An action that is itself built from smaller actions, like a recipe made of other recipes.

---

### Core Scripts
Shell scripts in `scripts/core/` that contain pure behavior with no substrate-specific dependencies. They run identically in GitHub Actions, Docker, or locally.

**Files:** `phase-runner.sh`, `metric-collector.sh`, `health-check.sh`

**Plain English:** The engine room — the actual work, stripped of all platform-specific wiring.

---

## D

### Degradation (Graceful)
See [Graceful Degradation](#graceful-degradation).

---

## E

### Entropy
In this system, "entropy" means the natural tendency of execution environments to become messy and unreliable over time: disks fill up, memory exhausts, networks fail, caches go stale. The [Reality Layer](#reality-layer) exists specifically to handle entropy.

**Plain English:** The mess that accumulates in any running system — dirt, clutter, and breakage.

---

## F

### Fractal Property
The observation that the same rhythm (init → prepare → execute → report → cleanup) repeats at every level: repository structure, workflow phases, action internals, and script structure. "Fractal" in mathematics means a pattern that repeats identically at every scale.

**Plain English:** The same shape at every zoom level. Learn the pattern once; recognize it everywhere.

---

## G

### GitHub Actions
GitHub's built-in automation platform. Workflows defined in `.github/workflows/` run automatically on events like pushes and pull requests.

**Plain English:** GitHub's built-in system for running automated tasks (tests, builds, deployments) when code changes.

---

### Graceful Degradation
Failing in a controlled, informative way instead of crashing silently. When an operation fails, the system records what failed, where, why, and how to fix it — then cleans up before stopping.

**Action:** `.github/actions/survival/graceful-fail`

**Plain English:** Failing politely, with a note explaining what went wrong and what to do next.

---

## I

### Invariant
Something that does not change, regardless of context or environment. In this system, behavior is invariant: a resource check behaves identically in GitHub Actions, Docker, or locally, even though the underlying tools may differ.

**Plain English:** A rule that is always true, no matter the circumstances.

---

### IVERO Pattern
Short for **Input → Validate → Execute → Report → Output**. The internal structure every action follows: declare inputs, check that they're valid, do the work, log what happened, and declare outputs.

**Plain English:** The standard "shape" of every action: gather requirements, verify them, do the job, report results, and return outputs.

---

## L

### Layer
A conceptual grouping of actions by purpose. There are three layers:
- **Reality Layer** (survival) — handles entropy and failure
- **Behavior Layer** (cadence) — defines consistent workflow phases
- **Teaching Layer** (observability) — makes state visible and comprehensible

**Plain English:** A category that describes what type of work an action does.

---

## M

### Manifest
A YAML configuration file in `manifests/` that defines data the system reads and obeys. Manifests hold phase definitions, metric definitions, thresholds, and cleanup policies — keeping configuration out of code.

**Files:** `phases.yml`, `metrics.yml`, `thresholds.yml`, `cleanup-policies.yml`

**Plain English:** A configuration file written as data (not code), like a settings panel the system reads.

---

### Metric
A measurable value about the system's state at a point in time: disk space available, memory free, CPU usage, phase duration. Metrics are collected at every phase and stored in `.metrics/*.json`.

**Plain English:** A number that tells you something concrete about how the system is doing right now.

---

## O

### Observability
How visible a system's internal state is. An "observable" system reports what it is doing, what it found, and why it made decisions. This system is observable by design: every action logs its state, and metrics are always available.

**Teaching Layer actions:** `metrics-collect`, `summary-generate`, `trace-context`, `failure-report`

**Plain English:** Being able to see inside the system at any time, like a dashboard with clear gauges.

---

## P

### Phase
A distinct, named stage in the [cadence](#cadence). The six phases are:
1. **Init** — establish context, check resources, set baseline
2. **Prepare** — restore cache, install dependencies, get ready
3. **Execute** — do the actual work (build, test, deploy)
4. **Report** — communicate what happened (summary, metrics)
5. **Cleanup** — free disk, prune caches, leave no mess
6. **Seal** — final health check, finalize state

**Plain English:** A chapter in the workflow's story — a named step with a clear purpose.

---

## R

### Reality Layer
The bottom layer of the [three-layer architecture](#three-layer-architecture). It handles the messy facts of real execution: low disk, low memory, network failures, corrupted caches.

**Actions:** `resource-check`, `cleanup-disk`, `prune-cache`, `validate-health`, `graceful-fail`

**Plain English:** The layer that deals with reality — fixing the inevitable problems that crop up in any real environment.

---

## S

### Seal
The final micro-step of a workflow — a health check that confirms the system ended in a known, good state. Conceptually: closing and sealing the envelope before sending it.

**Plain English:** The last "all clear" check before a workflow finishes.

---

### Status Propagation
The practice of passing the outcome of each step upward through the workflow, so the final report phase knows exactly what succeeded and what failed at every individual step.

**Plain English:** Keeping score at every step and passing that score to the report at the end.

---

### Substrate
The execution environment a workflow runs in: GitHub Actions runner, Docker container, or local machine. Core behavior is "substrate-agnostic" — it works the same regardless of which substrate it runs on.

**Plain English:** The platform or environment where a workflow actually runs.

---

### Survival
The name for the [Reality Layer](#reality-layer) directory (`.github/actions/survival/`). "Survival" refers to the system's ability to survive entropy — disk filling, memory exhausting, commands failing.

**Plain English:** The set of actions that help the system survive real-world chaos.

---

## T

### Teaching Layer
The top layer of the [three-layer architecture](#three-layer-architecture). It makes the system comprehensible: collecting metrics, generating summaries, creating trace context, and producing detailed failure reports.

**Actions:** `metrics-collect`, `summary-generate`, `trace-context`, `failure-report`

**Plain English:** The layer that explains what is happening and why — the system's narration of itself.

---

### Three-Layer Architecture
The organizing principle of this system's actions:

```
Teaching Layer  (observability)  → makes the system comprehensible
Behavior Layer  (cadence)        → keeps workflows consistent
Reality Layer   (survival)       → handles entropy and failure
```

Every action belongs to exactly one layer.

**Plain English:** Three types of concerns, each handled by its own set of actions.

---

### Threshold
A configured limit in `manifests/thresholds.yml` that triggers a system response. For example, when disk space drops below 5 GB (the critical threshold), the system must react.

**Plain English:** A line in the sand — when a metric crosses this value, something must happen.

---

### Trace Context
A unique identifier assigned to each workflow run that flows through every step. If something goes wrong, the trace context ID lets you correlate logs, metrics, and artifacts from the same run.

**Action:** `.github/actions/observability/trace-context`

**Plain English:** A serial number stamped on every run so you can connect all the pieces of the same execution together.

---

## W

### Workflow
A YAML file in `.github/workflows/` that defines an automated process triggered by a GitHub event (push, pull request, schedule). Workflows call [actions](#action) and follow the [cadence](#cadence) pattern.

**Plain English:** A recipe for automating a task, triggered automatically when code changes.

---

## Quick Reference

| Term | Plain English |
|------|---------------|
| Action | Reusable building block |
| Adapter | Platform plug adapter |
| Artifact | Saved output file |
| Cadence | Standard order of steps |
| Cache | Saved data to skip redo |
| Coherence | Everything still fitting together |
| Composite Action | Action built from other actions |
| Core Scripts | Platform-independent engine |
| Entropy | Inevitable system mess |
| Fractal Property | Same pattern at every zoom level |
| Graceful Degradation | Failing politely with explanation |
| Invariant | Rule that never changes |
| IVERO | Shape of every action |
| Layer | Category by type of work |
| Manifest | Settings file as data |
| Metric | Concrete measurement of system state |
| Observability | Ability to see inside the system |
| Phase | Named chapter in a workflow |
| Reality Layer | Handles real-world chaos |
| Seal | Final health check |
| Status Propagation | Passing scores upward to the report |
| Substrate | The platform/environment |
| Survival | Reality Layer actions |
| Teaching Layer | The system's self-narration |
| Threshold | Trigger limit |
| Trace Context | Run serial number |
| Workflow | Automated task recipe |

---

## Further Reading

- [MENTAL_MODEL.md](./MENTAL_MODEL.md) — How to think about the system as a whole
- [CONTRIBUTING.md](./CONTRIBUTING.md) — How to extend the system
- [PHILOSOPHY.md](../architecture/PHILOSOPHY.md) — Why the system is designed this way
- [PATTERNS.md](../architecture/PATTERNS.md) — Recurring structures and when to use them
