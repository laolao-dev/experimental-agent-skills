---
name: experience-distiller
description: Periodically review real development incidents and validated workflows across projects, classify them, and propose deduplicated Skill candidates. Use for experience-to-Skill reviews; not for ordinary debugging or automatic Skill installation.
---

# Experience Distiller

Turn accumulated engineering experience into reviewable, reusable behavior. Run on explicit request or a scheduled invocation; this Skill does not schedule itself. Scope the review period, projects, and available sources before collecting evidence.

## Sources and freshness

For each project, read the newest accessible Notion status first, then the project incident library, Development Log, Validation / Evidence, Decision Log, relevant PCE / Sister Memory, and the installed Skill inventory. Search by incident and capability, not only Skill name. Record source location, event date, verification status, and whether the source describes history or current state. If a source is unavailable, mark that gap; do not infer its contents. Verify current project state from live sources before making current-state claims. Chat history and memory can locate evidence but cannot alone establish current status.

### Evidence Retrieval Gate

If the first search misses a historical incident, do not immediately mark it `UNVERIFIED`. Search the current project's canonical Notion page, the project incident library, and relevant Development Log, Validation / Evidence, and Decision Log. Use the original words plus a bounded fallback of synonyms, root-cause terms, and user-visible symptoms across those sources. For example, an OAuth popup may be indexed under `auth/connect`, `hook`, `SessionStart`, `ghost behavior`, or `old config`. Limit retrieval to one direct-search round and one synonym/root-cause fallback round, then stop. If evidence is still absent, report `UNVERIFIED` with `SEARCHED_SOURCES=`, `SEARCH_TERMS=`, and `MISSING_EVIDENCE=`. Unavailable sources must be named rather than silently counted as searched.

For every located historical case, record `EVIDENCE_EXISTS=`, `HISTORICAL_EVENT_STATUS=`, and `CURRENT_STATUS_VERIFIED=` separately. A resolved historical event proves that it occurred and was resolved at the documented time; it does not prove the current machine still has the cause or symptom. Set current verification to `NO` unless the current task calls for and actually performs a live check.

## Classify each distinct finding

Assign exactly one primary class:

- `ONE_OFF`: isolated issue with no demonstrated reusable procedure.
- `PROJECT_SPECIFIC`: useful only inside one project's rules or log.
- `GLOBAL_RULE`: cross-project guardrail better kept as a general rule than a standalone workflow.
- `SKILL_CANDIDATE`: a distinct, repeatable execution pattern meeting the gate below.
- `EXISTING_SKILL_UPDATE`: an installed Skill already owns the capability; propose a narrow update there.

Merge duplicate reports of the same underlying event before counting recurrence. Distinguish a proposed rule from a verified practice. Do not promote a one-time ordinary failure merely because its story is memorable.

## Candidate gate

Promote to `SKILL_CANDIDATE` only when evidence shows (A) at least two real occurrences of the same problem; (B) one high-risk or high-cost event whose recurrence is unacceptable, such as a secret leak, destructive change, wrong push or release, large API spend, false current-state claim, or irreversible system change; or (C) a workflow actually verified successful and clearly likely to recur. Then define all of: trigger, exclusions, inputs, executable workflow, stop conditions, verification, rollback or explicit no-mutation recovery, and at least one real source case. If any field is missing, retain it in the experience library and name the missing evidence. A gate pass is a candidate, not permission to create a Skill.

## Deduplicate by capability

Inspect all currently installed Skills, including system and plugin Skills available in the environment. Compare triggers, inputs, workflow, outputs, and boundaries; names alone are insufficient. If one canonical Skill covers at least about 70% of the proposed capability, classify it as `EXISTING_SKILL_UPDATE` and describe the smallest useful change. Flag overlapping Skills in `DUPLICATE_SKILLS_FOUND`; do not create another owner for the same capability. Keep project-specific procedures with their project unless a cross-project pattern is evidenced.

Memory / PCE records preferences, history, context, decisions, and provenance. A Skill records execution steps, tool order where material, gates, verification, and recovery. A Skill may instruct the runner to read relevant current memory; never paste a personal memory dump into it.

## Authority and stop conditions

Default to read-only review and proposals. Create or edit a Skill only when the user explicitly authorizes that mutation; do not install, overwrite, delete, push, publish, or contact others on the strength of a candidate alone. Stop promotion when provenance, recurrence, successful verification, required workflow fields, or deduplication cannot be established. State the exact gap and next read-only check. Respect stricter project gates and authorization limits. For read-only distillation, rollback is `not applicable`; for any later Skill edit, preserve the prior file or diff and validate the changed Skill before reporting success.

## Output

Return only these fields, with concise evidence references and `NONE` or `UNVERIFIED` where appropriate. Put each finding in exactly one class; candidate entries include their gate and missing fields, if any. Include the evidence-retrieval and historical/current-status fields within the relevant finding; when `UNVERIFIED`, include its searched sources, terms, and missing evidence there. Do not call old evidence current status.

```text
EXPERIENCE_DISTILLATION_STATUS=
NEW_SKILL_CANDIDATES=
UPDATED_SKILL_CANDIDATES=
GLOBAL_RULE_UPDATES=
PROJECT_SPECIFIC_ITEMS=
ONE_OFF_ITEMS=
DUPLICATE_SKILLS_FOUND=
HIGHEST_VALUE_CANDIDATE=
WHY=
EVIDENCE=
NEXT_STEP=
```
