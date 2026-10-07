# summary.md — common patterns, disagreements, notable ideas

## The convergence (what all 4 outputs agree on)
These appear in **every** output, independent of temperature or harness. That
agreement is the signal:

1. **Layered memory** — working (KV/latent) + episodic (append-only event log)
   + semantic (vector + knowledge/property graph with confidence & provenance)
   + **procedural (a skill library)** + parametric (weights/adapters).
   The skill library is the standout: every system treats "learned procedure"
   as a first-class, versioned, tested artifact.

2. **LLM-as-proposer, NOT as system-of-record.** Every system puts an
   independent **verifier** and/or **world model** between the model's proposal
   and any real action. The model proposes; something else checks.

3. **Plan-as-DAG with preconditions/effects.** Planning is structured (MCTS /
   beam / best-first over an action DAG), not free text. Replanning on
   divergence is universal.

4. **Sandboxed, permissioned tool execution** with idempotency, rollback, and
   human-in-the-loop for high-risk effects.

5. **Event-sourced persistence** — append-only logs + materialized state,
   reproducible builds, checkpointing.

6. **Learning = distill verified success into reusable skills**, plus
   self-generated curriculum from failures/gaps. Not blind online gradient
   updates in production.

## The disagreements (where they diverge)
- **World model form.** system_01 leans on a *differentiable latent dynamics*
  model; system_02 on an *explicit causal graph*; system_03 on a *library of
  falsifiable micro-simulators* (explicitly against one monolithic sim).
- **Original insight (the differentiator):**
  - system_01 — **Transactional Cognition**: every inference/act/memory-write is
    a DB transaction with pre/postconditions; the core unit is a *commitment*.
  - system_02 — **Effect-Typed Planning**: reasoning compiles to a typed effect
    system; safety is *type-checking effects*, not post-hoc monitoring.
  - system_03 — **Verifiability Pressure**: only promote hypotheses with
    cheaply checkable predictions; that is what defeats reward hacking.
  - system_04 (agent harness) — **Reliable loop > bigger brain**: the compounding
    act-verify-distill-reuse loop is the real source of general competence.
- **Self-improvement mechanism.** system_01: RL (PPO/GRPO) on a shadow
  model. system_02: fine-tune the proposer on verified plans; PR-like pipeline
  for self-modification. system_03: *bounded* gradient / "policy surgery" on a
  frozen base + per-domain adapters. All agree the update must be
  verification-gated.

## Notable ideas worth lifting
- **Verification budget as a first-class cost term** (system_03's
  score = utility − risk − verification-cost). Treats checking work as a
  scarce resource to be allocated, not free.
- **Conservation of verifiable competence** (system_02): a skill is not
  learned until it ships a regression test + a rollback.
- **Effect typing** (system_02): classify actions by side-effect class
  (read/write/spend/send/delete/train/deploy) and enforce budgets by type.
- **Skill-as-MCP-server** (system_01/03): compiled tool sequences become
  callable services, not just text.
- **Falsifiable micro-simulators** (system_03): many small, checkable domain
  sims beat one big uncheckable one.

## Honest note on breadth
These 4 outputs are 2 distinct systems (Qwen-27B raw × 3 samples, agent harness
× 1). The *agreement* is therefore partly within-family. A full 8-system run
would test whether Claude/Gemini/Grok/DeepSeek independently land on the same
skeleton (especially: is "skill library + independent verifier" truly universal
or a Qwen artifact?). That is the main open question this packet sets up.
