# prompts.md — exact prompt(s) used per system

## Comparable base prompt (used for every system)
> You are an AI research system. Propose a concrete, technical AGI (Artificial
> General Intelligence) architecture. Cover: memory architecture,
> reasoning/planning loop, learning/self-improvement mechanism, tool use and
> action execution, world-model/representation layer, safety and governance
> layer, evaluation and benchmark strategy, persistence/runtime,
> multi-agent/orchestration design, engineering feasibility, and at least one
> original non-obvious insight. Be specific and concrete, not philosophical.

## Per-system adaptation (documented per the bounty requirement)

### system_01 — Qwen 3.8-27B (raw, LM Studio local API)
- Endpoint: `http://127.0.0.1:1234/v1/chat/completions` (OpenAI-compatible, local)
- Model: `qwen/qwen3.8-27b` (Q4_K_M GGUF, ~17.7 GB, loaded in VRAM)
- `temperature`: 0.7, `max_tokens`: 1400
- Adaptation: **none** - base prompt verbatim.
- Note: Qwen 3.8 is a thinking model; the proposal text is returned in the
  `reasoning_content` field. Saved verbatim (that is the raw output).

### system_02 — Qwen 3.8-27B (raw, LM Studio) independent sample
- Same endpoint/model. `temperature`: 0.2 (low - deterministic end of the
  distribution), `max_tokens`: 1400. Adaptation: none.

### system_03 — Qwen 3.8-27B (raw, LM Studio) independent sample
- Same endpoint/model. `temperature`: 0.9 (high - exploratory end),
  `max_tokens`: 1400. Adaptation: none.

### system_04 — Hermes agent layer (Qwen 3.8-27B weights + agent harness)
- The base prompt was given to the Hermes-Cairn agent (same model weights,
  different harness: multi-tool loop, persistent memory, delegation, skills).
- The agent authored its proposal as the entry. This is a distinct "system"
  in the architectural sense (a tool-actuated, memory-persistent agent), not a
  distinct model family. Labeled honestly as such in `sources.md` and README.

## Systems NOT reached (documented, NOT fabricated)
The posting lists examples: ChatGPT/OpenAI, Claude, Gemini, Grok, DeepSeek,
Llama-based, Mistral-based, Perplexity. None of these were reachable on the
machine that produced this packet (no API keys, no endpoints). Per the
acceptance criterion "If a model or tool could not be accessed, state that
explicitly instead of inventing output" - they are listed here as
**not-accessed**, with no invented output. See README.md.
