# Experimental agent skills

Three small instruction workflows developed while working on agent tooling:

- **Experience Distiller** reviews evidenced incidents and proposes reusable workflow candidates.
- **Skill Router** selects the smallest relevant set of available skills, respecting their actual triggers and exclusions.
- **Personal Context Engine Diagnostics** performs read-only diagnosis before a misleading status report leads to destructive recovery.

## Install and use

Copy an individual directory from `skills/` into your agent's supported skill directory, commonly `~/.codex/skills/`. Inspect it before installation. This repository does not modify your installed skills or hook configuration. The workflows need an agent capable of reading SKILL.md; optional memory and project-source tools must be supplied by your environment.

Example requests: “Use Experience Distiller to review this month's verified incidents”; “Use Skill Router to choose the minimum skills for this task”; “Use Personal Context Engine Diagnostics to investigate this status discrepancy using read-only evidence.”

## Validation and limitations

The installed originals previously passed a structural validator. This publication checks the cleaned copies' frontmatter, file structure, and removal of private references; see VALIDATION.md. It does not demonstrate deterministic skill activation or a complete automatic skill-evolution loop. Diagnostics cannot confirm a live fault without authoritative local evidence. Configure your own project sources and incident library.

Private project links and instance-specific incident details have been removed. No additional redistribution license is granted; see RIGHTS.md. [Issues](https://github.com/laolao-dev/experimental-agent-skills/issues) are welcome.
