# synthesis.md — a proposed combined architecture

Combining the strongest, mutually-reinforcing ideas from the 4 outputs into
one concrete design. Goal: concrete enough to inform implementation, not a
philosophical essay.

## Core principle
A **verification-gated, effect-typed, skill-compounding agent runtime**. The
model proposes; a typed effect ledger checks; verified success compiles into
reusable, tested skills. No capability is added unless it ships a check.

## The 6 load-bearing components

### 1. Layered memory (union of all 4)
- **Working:** bounded KV/latent context + active goal, plan, constraints, facts.
- **Episodic:** append-only event log (W3C causal trace IDs), every
  observation/action/tool-output/model-state. This is the training substrate.
- **Semantic:** property graph of typed entities/relations/facts, each with
  confidence, provenance, TTL, evidence IDs. State = projection over events
  (materialized views).
- **Procedural:** a **skill library** — each skill is an executable policy:
  preconditions, action template, code/MCP service, params, examples, a
  **mandatory regression test**, a risk class, cost, provenance, and a rollback
  path. Versioned like code.
- **Parametric:** frozen base model + per-domain adapters (LoRA), updated
  offline only from verified examples, selected by a router.

### 2. Effect-typed planning (from system_02, kept by all)
- Every action has an **effect class**: read / write / spend / send / delete /
  train / deploy, with amount, reversibility, blast radius.
- Planner emits a **plan DAG** (preconditions + effects per step).
- A **type-checker / policy engine** rejects plans whose effects exceed budget
  or violate policy. Safety is *static*, not post-hoc monitoring.

### 3. The verification loop (the differentiator)
- Independent **verifier** (separate model + formal checks) validates each
  proposal: constraints, consistency, predicted effects, budget.
- **Verification cost is a first-class term** in the plan score:
  `score = expected utility − risk penalty − verification cost`.
- **Verifiability Pressure rule:** a hypothesis/skill is promoted only if it
  has a *cheaply checkable* prediction (a falsifier). This is the anti-reward-
  hacking mechanism and the compounding engine.

### 4. World model (library, not monolith)
- **Fast tier:** embedding-based transition predictors for short-horizon
  rollout.
- **Slow tier:** a **library of falsifiable micro-simulators** — one per domain
  (code execution, browser/DOM, APIs, physics) — plus an explicit causal graph
  for cross-domain reasoning. Counterfactual-queryable.
- Prefer many small checkable simulators over one big uncheckable one.

### 5. Sandboxed, transactional execution
- Tool registry: typed schemas, side-effect class, cost, latency, auth scope.
- Executor: gVisor/Firecracker sandbox, egress allowlist, secrets vault,
  rate limits, idempotency keys.
- Every action is a **transaction** with pre/postconditions; on failure,
  rollback. Human-in-the-loop gate for high-risk effect classes.

### 6. Skill-compounding self-improvement
- After each episode: generate trace → extract candidate skills → **gate on
  regression test + canary** → merge to skill library (PR-like).
- Self-generated curriculum from world-model gaps and failed traces.
- Bounded gradient/"policy surgery" on adapters from verified examples only.
- **Conservation of verifiable competence:** no skill without a test + rollback.

## Why this combination wins
- Effect typing + verification-cost scoring (system_02/03) makes safety
  *computable* and bounded.
- Verifiability Pressure (system_03) + conservation of competence (system_02)
  together turn "learning" into "verified, reversible, compounding" — which
  directly answers the failure mode that scares people about AGI (reward
  hacking / unbounded self-modification).
- Skill-as-service (system_01/03) + reliable loop (system_04) is already
  runnable in production today — it is a real, deployed pattern, not a sketch.

## Minimum viable version (build order)
1. Episodic event log + semantic fact graph with provenance.
2. Skill library with mandatory test + rollback.
3. Effect-class registry + policy/type-checker gate.
4. One micro-simulator (code execution) as the world-model tier.
5. Verifier (separate model) + verification-cost scoring.
6. Adapter-based offline self-improvement from verified episodes.

## What this does NOT solve (honest)
- The open AGI questions (generalization beyond the trained distribution,
  robustness to distribution shift, alignment of open-ended goals) are not
  solved by architecture alone. This is a *reliable, safe, compounding*
  runtime, not a guarantee of general intelligence.
