# Claude Opus 5.5 (xhigh) — ProgramBench submission

- **Model:** `claude-opus-5-5`, reasoning effort **xhigh**, max 65,536 output tokens per call
- **Scaffold:** mini-SWE-agent v2.4.5 (single-agent), open source; step limit 1,000, no cost
  limit, wall-time limit 6 h (71 tasks) or 48 h (124 tasks)
- **Protocol:** no-internet + `task_cleanroom_v6` images. The agent sees only the compiled binary
  (source removed, `.git` reset), reimplements it from scratch by interacting with it, and never
  has network access (`--network none` at the container level).
- **Score:** ~0.877 (mean per-instance test pass-rate over the fixed 200, missing = 0; the
  leaderboard recomputes authoritatively from `_stats/score.json`). Only tests listed in the
  benchmark's `tests.json` are scored.
- **Inference dates:** 2026-10-05 to 2026-10-11

## Task accounting

195 tasks are evaluated and included here. Five are scored 0 and have no directory:
`eliukblau__pixterm`, `facebook__zstd`, and `ip7z__7zip` produced no submission after repeated
attempts; `osgeo__proj` and `tukaani-project__xz` were still running at the release cutoff.

Rollouts that failed on provider/infrastructure errors were retried, and each task ships the
trajectory and solution of the attempt that was scored. Six tasks (`alexpovel__srgn`,
`bellard__quickjs`, `chirlu__sox`, `rochacbruno__marmite`, `rvben__rumdl`,
`sayanarijit__xplr`) hit the ~1M-token context limit without submitting; their workspaces were
snapshotted and evaluated as-is, and their trajectories end without an exit status.

## Extra stats

Cost, model-call, and output-token statistics are generated from the published trajectories by
the scripts under `_scripts/`. Cost is mini-SWE-agent's per-task `instance_cost`, priced at
Anthropic's list rates for Claude Opus 5.5: $4 per million uncached input tokens, $0.20 per
million cached input tokens, and $20 per million output tokens.

## Redactions

Harness metadata naming internal hosts and users is replaced with `[REDACTED]` in each
trajectory's `info.config` (`safety_identifier`, `litellm_model_registry`, `output_path`), and
host paths in harness tracebacks are rewritten to `/harness/`. Model and tool turns are
unmodified.

Each test is run up to 3× (pytest-rerunfailures); only the final attempt is scored (flakiness defense).
