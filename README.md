# Experimental Agent Skills

Three experimental `SKILL.md` instruction workflows for `SKILL.md`-compatible coding agents: task-to-skill routing, incident-to-workflow distillation, and read-only context diagnostics.

**Status:** Experimental, not maintained

## Why

As personal coding agents accumulate dozens of custom skills, naive keyword matching loads irrelevant instructions or triggers destructive recovery steps on stale status reports. Writing explicit trigger gates, hard exclusions, and read-only diagnostic checklists helps bound what an agent loads and executes. This repository shares three sanitized `SKILL.md` specifications extracted from local agent-tooling experiments.

## Quickstart

This repository contains static Markdown skill definitions (no executable runtime). Inspect the files directly or copy an individual folder into an agent skill directory (such as `~/.codex/skills/`):

- [skills/skill-router/SKILL.md](skills/skill-router/SKILL.md) — selects the minimum sufficient set of installed skills using explicit trigger, exclusion, project-scope, and overlap gates.
- [skills/experience-distiller/SKILL.md](skills/experience-distiller/SKILL.md) — reviews evidenced debugging/incident notes and proposes candidate reusable workflows without auto-installing them.
- [skills/personal-context-engine-diagnostics/SKILL.md](skills/personal-context-engine-diagnostics/SKILL.md) — enforces read-only evidence checks before acting on a degraded memory/status report.

## Limitations

- **Instruction-only prompts, not deterministic code:** Compliance depends entirely on the host model reading and following `SKILL.md`; deterministic skill-to-skill invocation is not supported by these files, and live model-led execution is not benchmarked here (see [VALIDATION.md](VALIDATION.md) for structural frontmatter checks).
- **Environment-dependent references:** Several boundary examples inside the prompts name local skills or memory services (`Supermemory`, `PCE`, Notion status pages) that must be adapted or supplied by your own environment.
- **No automated skill-evolution loop:** Candidate generation and installation remain manual human-reviewed steps.

## License

No license yet (see [RIGHTS.md](RIGHTS.md)).
