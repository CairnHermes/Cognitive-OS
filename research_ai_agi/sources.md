# sources.md — models, providers, dates, links, human edits

## Systems used (real, accessed)
| # | System | Model | Provider/Tool | Access date | Raw output | Human edits |
|---|--------|-------|---------------|-------------|------------|-------------|
| 01 | Qwen 3.8-27B (raw) | qwen/qwen3.8-27b (Q4_K_M GGUF, ~17.7GB) | LM Studio local API, `127.0.0.1:1234` | 2026-10-05 | raw_outputs/system_01_*.txt (6,480 chars) | whitespace only |
| 02 | Qwen 3.8-27B (raw, sample) | qwen/qwen3.8-27b | LM Studio local API | 2026-10-05 | raw_outputs/system_02_*.txt (6,555 chars) | whitespace only |
| 03 | Qwen 3.8-27B (raw, sample) | qwen/qwen3.8-27b | LM Studio local API | 2026-10-05 | raw_outputs/system_03_*.txt (6,596 chars) | whitespace only |
| 04 | Hermes agent layer | qwen/qwen3.8-27b (same weights, agent harness) | Hermes Agent (multi-tool, persistent memory, delegation) | 2026-10-05 | raw_outputs/system_04_*.txt (3,798 chars) | authored by the agent |

## Notes on honesty
- **One model family** (Qwen 3.8-27B), accessed two distinct ways: raw API and
  as the agent's own brain/harness. That is **2 distinct systems** (different
  architectures: a single forward pass vs a tool-actuated, memory-persistent
  agent), not 8.
- system_02/03 are independent temperature samples (0.2, 0.9) of the same
  system — labeled as samples, not as separate systems.
- **No output was fabricated** for any system that was not accessed.

## Systems NOT accessed (listed for completeness, per the posting)
ChatGPT/OpenAI, Anthropic/Claude, Google/Gemini, xAI/Grok, DeepSeek,
Llama-based, Mistral-based, Perplexity. None were reachable on the producing
machine (no API keys / no endpoints). Per the acceptance criterion: "If a model
or tool could not be accessed, state that explicitly instead of inventing
output." They are therefore documented here as **not-accessed**, with no
invented content.

## How to reach 8+ systems (the missing step)
Any of the following, each would add one real system to the packet:
- An API key for any of: OpenAI, Anthropic, Google, xAI, DeepSeek, Mistral,
  Perplexity, Together, Fireworks.
- A web UI for any of the above (logged in), to capture a generated proposal.
- A second local model (e.g. Llama-3, Mistral, DeepSeek-R1) loaded in LM Studio
  or Ollama.

Once any of these are reachable, the same prompt (see prompts.md) can be run
and the entry added to raw_outputs/ + comparison.csv + this table. The pipeline
(prompts → raw → comparison → summary → synthesis → sources) is complete and
ready to accept the additional systems.

## Privacy / safety compliance
- No API keys, account tokens, paid-account screenshots, hidden system prompts,
  or proprietary content are included in this packet.
- No model or tool system instructions were leaked.
- Only public, authorized, personally-accessible systems were used.
- Short generated outputs are included; the submitter has the right to use
  them (locally generated).
