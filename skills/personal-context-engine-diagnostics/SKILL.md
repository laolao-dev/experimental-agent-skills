---
name: personal-context-engine-diagnostics
description: Perform read-only deep diagnostics when Personal Context Engine status appears false, canonical and runtime state conflict, or a reported failure could prompt destructive recovery. Do not use for routine PCE status, memory recall, or Supermemory cloud/account questions.
---

# Personal Context Engine Diagnostics

Diagnose Personal Context Engine (PCE) state before anyone treats a status view as proof of data loss or runtime failure. This is a PCE-specific, read-only workflow. `STATUS_VIEW_IS_NOT_SOURCE_OF_TRUTH`.

## Trigger and exclusions

Use when PCE reports `canonicalCount=0`, missing canonical DB or binary, binary hash mismatch, disabled LLM, unhealthy runtime, missing listener, firewall/runtime-boundary anomaly, or conflict between canonical, derived, runtime, and status views. Prioritize it when the reported problem could lead to restore, reinstall, reset, DB or binary replacement, config wipe, or migration rollback.

Do not use for routine PCE status, memory recall/content queries, historical-event summaries without a live diagnosis request, Supermemory cloud account/OAuth/connection or ordinary hook checks, unrelated Windows/runtime faults, or ordinary project bugs. The PCE personal-context Skill owns normal context operations; Supermemory Status owns its connection status. Add Safe Long Task only if the actual diagnosis has material duration or resource risk.

## Sources and evidence boundary

Locate the PCE canonical Notion page first for architecture, formal paths, constraints, and historical cases. Then establish the current formal repo/runtime entry, canonical DB, pinned binary and expected hash, config/environment, backups, process, listener/firewall, and the status output under investigation. Prefer current authoritative config and runtime evidence for live conclusions; never reuse historical PID, port, count, hash, or path as a current fact without checking it. Treat Notion history as provenance, not live state. Do not expose secret values while inspecting environment or config.

Before a destructive recovery, independently verify canonical data and runtime state. A misleading status view can coexist with healthy data; historical cases do not establish a current fault.

## Read-only diagnosis, in order

1. **Execution context:** Record the exact command, cwd, user, `LOCALAPPDATA`, `PCE_DATA_ROOT`, relevant environment-variable presence or non-secret values, executable resolution, and PCE entry actually invoked. Resolve the formal data root from current config. Stop if entry or data root remains unclear.
2. **Process:** Independently inspect whether the expected PCE/local runtime process exists, its PID, start time, executable path, and parent when useful. A status claim is only a lead.
3. **Listener and boundary:** Identify the expected port from current config, observed listener, local address, owning PID, and relevant firewall rule only when the symptom concerns reachability or boundary. Relate the listener PID to the runtime process; do not infer health from an unrelated listener.
4. **Binary identity:** Check expected and actual binary paths, file existence, executable resolution, and SHA-256 against the current pinned hash source. A hash mismatch with an unverified expected value remains unresolved.
5. **Canonical DB:** Resolve the formal DB path before opening it. Use a genuinely read-only method to inspect existence, schema, canonical count, critical baseline records, timestamps when meaningful, and lifecycle/current-state consistency. Avoid commands that open a writable SQLite connection, create WAL/journal files, migrate schema, or alter DB metadata. Stop if the path or read-only behavior cannot be established.
6. **Backup:** Read metadata for available backups: existence, timestamp, schema/version, and evidence of a known usable snapshot. Backup presence alone never justifies restore.
7. **Config and environment:** Compare PCE/runtime/LLM config, data-root overrides, environment, cwd, resolved executable, and legacy entry. Check old plugin/hook/config interference only when evidence points there. Determine whether the suspect status command used a different entry or data root.
8. **Compare sources:** Separately record `STATUS_VIEW`, `CANONICAL_STATE`, `RUNTIME_STATE`, `DERIVED_STATE`, and `BACKUP_STATE`. Explain agreements and conflicts; derived retrieval is not canonical truth.
9. **Diagnose:** Use only evidence-supported labels such as `FALSE_NEGATIVE_STATUS`, `WRONG_ENTRY`, `WRONG_DATA_ROOT`, `RUNTIME_ACTUALLY_DOWN`, `BINARY_MISSING_CONFIRMED`, `CANONICAL_DB_MISSING_CONFIRMED`, `CONFIG_MISMATCH`, `ENVIRONMENT_OVERRIDE`, or `UNVERIFIED`. State `DATA_LOSS` and risk only to the strength of the independent checks.
10. **Destructive-action gate:** Set `DESTRUCTIVE_ACTION_ALLOWED=NO` in this Skill's output. Never execute restore, reinstall, reset, DB deletion/replacement, binary replacement, config wipe, migration rollback, firewall/config/DB changes, or restart as part of this diagnostic. A later recovery task needs independently verified fault, checked backup/rollback, and explicit authorization for its exact action.

## Stop and read-only integrity

Stop and preserve evidence when the PCE entry, data root, or canonical DB path is unknown; evidence conflicts or status/runtime differences cannot yet be explained; a binary hash anomaly lacks an authoritative expected hash; backup state is unknown before contemplated recovery; or continuation needs mutation or authorization. Do not widen scope merely to produce a confident verdict. Report `UNVERIFIED` for unresolved diagnosis.

Where feasible, compare pre/post canonical DB digest, config hash, binary hash, and relevant Git/worktree state. A hash read is itself only safe after identifying the correct file. If an allegedly read-only command mutates state, stop immediately, report `READ_ONLY_MUTATION_DETECTED=YES`, retain before/after evidence, and do not stack more changes. The normal rollback is none because no mutation is intended; any unexpected mutation needs a separate, authorized recovery decision.

## Output

For a live diagnostic, return exactly these fields. Use `NOT_CHECKED` or `UNVERIFIED` rather than filling gaps from history. For a historical replay, label it as such in `PCE_DIAGNOSTICS_STATUS` and `EVIDENCE`, and do not imply current verification.

```text
PCE_DIAGNOSTICS_STATUS=
TRIGGER_REASON=
EXECUTION_CONTEXT=
PCE_ENTRY=
DATA_ROOT=
PROCESS_STATE=
LISTENER_STATE=
FIREWALL_STATE=
BINARY_STATE=
BINARY_HASH_STATE=
CANONICAL_DB_STATE=
CANONICAL_COUNT=
SCHEMA_STATE=
BACKUP_STATE=
CONFIG_STATE=
ENVIRONMENT_STATE=
STATUS_VIEW=
CANONICAL_STATE=
RUNTIME_STATE=
DERIVED_STATE=
DIAGNOSIS=
FALSE_NEGATIVE_CONFIRMED=
DATA_LOSS=
DATA_LOSS_RISK=
READ_ONLY_MUTATION_DETECTED=
DESTRUCTIVE_ACTION_ALLOWED=NO
EVIDENCE=
BLOCKER=
NEXT_SAFE_ACTION=
```
