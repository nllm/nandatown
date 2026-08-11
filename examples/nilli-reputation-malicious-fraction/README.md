# reputation - Nilli change

I picked malicious_fraction (0.2 → 0.8) because there are two settings that declare how many malicious agents there are, and I wanted to know which one had priority.

From reading the code, my hypothesis was that the malicious count would stay at 4 since `agents.roles` overrides `task.config.malicious_fraction`, but I wasn't fully sure — I was using Claude Code to find the relevant snippets and wanted to rule out a hidden config I might've missed.

Comparing the traces confirmed it: malicious agents stopped at 4 (`malicious-0` through `malicious-3`), just like the original run, instead of the ~16 I'd expect if malicious_fraction actually drove the count.

This surprised me — I'd expect every setting in a given example to actually change behavior, and if one overrides another, I'd expect that documented; it wasn't flagged anywhere, which confirmed Claude Code had found the only two relevant parts: `agents.roles.count` and `task.config.malicious_fraction`.

The other surprise: the output files were identical, which taught me NANDA produces deterministic output — the trace matched exactly because the YAML genuinely hadn't changed (double-checked in VS Code) — and re-running with a change to `agents.roles.count` instead did change the count, this time to 8 malicious agents in `reputation_80p8.jsonl`.

**Idea for a future scenario:** a supply-and-demand simulation extending `marketplace` — configurable seller pricing strategy (fixed, undercutting, surge) and buyer price sensitivity, varying the buyer-to-seller ratio to watch demand redistribute across vendors — since the existing `marketplace` scenario doesn't expose pricing as a configurable knob.

**Files:**
- `nilli_rep.yaml` — scenario config used for the runs
- `reputation.jsonl` — baseline (malicious_fraction 0.2, roles: 4 malicious)
- `reputation_80p.jsonl` — malicious_fraction 0.8, roles unchanged (identical to baseline)
- `reputation_80p8.jsonl` — roles.malicious.count raised to 8
