---
name: skill-router
description: Select the minimum sufficient installed Skills for a task by checking their real capabilities, triggers, exclusions, project scope, and risk. Use when the user asks which Skills to use or requests Skill routing; do not create or modify Skills.
---

# Skill Router

Route a task to the smallest useful set of currently available Skills. The user request is the intent source. Routing is read-only and grants no new permission to run the selected workflow or perform external actions.

## Discover the live inventory

Read the Skills catalog supplied by the current runtime and inspect relevant `SKILL.md` files. Check personal, system, plugin, and any other discoverable Skills. Filesystem enumeration alone is insufficient: a runtime-listed Skill may live in a plugin or be visible only through the runtime. Mark each relevant entry `INSTALLED_CONFIRMED` when its file is accessible, `AVAILABLE_RUNTIME_ONLY` when advertised but its file cannot be inspected, or `UNVERIFIED` when only an old record mentions it. Never promote `UNVERIFIED` to available.

For each plausible Skill, extract its actual name, source, description, triggers, exclusions, project scope, risk scope, input requirements, and output type. Use current metadata, then read full instructions only for plausible matches. At minimum check the current availability and capability of Experience Distiller, Agent Boundary Lab Workflow, Safe Long Task, Hatch Pet, Image Gen, OpenAI Docs, and Supermemory Status. Include Review Agent or other system Skills when actually discoverable. Do not infer capability from a name.

## Establish task context

Identify the task, requested output, project, allowed scope, and material risks. Read a project's canonical Notion Current Status, phase, blocker, formal decisions, and allowed scope only when the task belongs to that project and those facts affect routing. Read PCE / memory only when user history, preferences, or earlier workflow affect routing. Memory can explain historical context but cannot override canonical current status. If project identity is uncertain, do not guess a project-specific Skill.

## Routing gate

For each plausible Skill, apply these checks in order:

1. `TRIGGER_MATCH`: Does the requested action actually meet the Skill's trigger? A keyword alone is insufficient.
2. `EXCLUSION_CONFLICT`: A stated exclusion is a hard reject.
3. `PROJECT_MATCH`: A project-bound Skill needs the matching project or a clearly covered analogous workflow. The word “Agent” alone does not select ABL Workflow.
4. `RISK_MATCH`: Add a risk-governing Skill only for a material risk it owns. A short ordinary command does not select Safe Long Task.
5. `CAPABILITY_OVERLAP`: Prefer the canonical owner when candidates solve the same problem; combine only complementary capabilities.

An optional internal ranking can score trigger 0–3, project 0–2, risk 0–2, context value 0–2, and overlap penalty 0 to −2. Exclusion is a hard reject. Scores are sorting aids, never probabilities or a substitute for a plain-language reason.

Apply `MINIMUM_SUFFICIENT_SKILLS`: prefer one precise Skill, then two complementary Skills, then three only when necessary. Three is the normal maximum. Give the execution order when combining. A resource-budget Skill precedes a domain execution Skill when it must establish limits first. Do not load many Skills for reassurance. If none matches an ordinary task, use `SELECTED_SKILLS=NONE` and `SKILL_GAP_DETECTED=NO`; do not force a Skill. Mark a gap only for a real, repeated or material uncovered capability, and do not create a candidate yourself.

Examples of boundaries: Experience Distiller handles historical experience-to-Skill review, not ordinary debugging. Hatch Pet handles the pet asset pipeline, not every image edit; Image Gen handles bitmap creation or editing, not a mere image path. OpenAI Docs handles OpenAI product/API questions, not generic software work. Supermemory Status handles Supermemory connection, cloud hook, or account status; do not route PCE canonical-count or runtime diagnostics to it unless its current instructions actually cover that work. Route PCE context or status to a PCE-specific capability when available, and report a diagnostics gap if no Skill owns it.

## Runtime and authority limits

The runtime may expose Skill metadata and let the agent read another `SKILL.md` in the same task. That is model-directed use, not proof of a deterministic activation API. Report `AUTO_SKILL_ACTIVATION_SUPPORTED=PARTIAL` unless a real invocation chain in the current runtime proves more; report `NO` or `UNVERIFIED` when appropriate. If continuing the task after routing, use the selected Skill's actual instructions through the mechanism available here. Do not claim an activation merely because a Skill was named in the output.

Router never creates candidates, edits Skills, writes memory, or decides project Current Status. A selected Skill does not authorize push, merge, release, publication, payment, submission, or contacting people. Keep the user's exact permission boundary.

## Output

For a routing request, report only these fields. Include compact availability states in `AVAILABLE_SKILLS_CHECKED` and reasons for important rejects. `NEXT_ACTION` distinguishes a recommendation from a Skill actually read and followed.

```text
SKILL_ROUTER_STATUS=
TASK_CLASS=
PROJECT=
PROJECT_CONTEXT_REQUIRED=
AVAILABLE_SKILLS_CHECKED=
SELECTED_SKILLS=
EXECUTION_ORDER=
REJECTED_SKILLS=
WHY_SELECTED=
WHY_REJECTED=
MEMORY_REQUIRED=
NOTION_REQUIRED=
AUTO_SKILL_ACTIVATION_SUPPORTED=
SKILL_GAP_DETECTED=
NEXT_ACTION=
```
