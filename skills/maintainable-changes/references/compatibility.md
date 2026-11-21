# Decide who is owed compatibility

For each changed name or shape, locate its consumers in space **and in time**. A private function can be local to one release. A JSON key built by that same function can outlive the process for months. Reason about those two surfaces separately.

| Evidence in this project | Decision to take |
| --- | --- |
| A private helper, all uses in one deployment unit, no string-based resolution or stored representation | Rename it and update its callers together. Remove the superseded name and any test whose only promise was that private name. |
| An unreleased feature with disposable fixtures and no deployed writers or promised external format | Change the representation in place and update the fixtures. Check that a fixture is synthetic before treating it as disposable. |
| A published SDK method, documented API, CLI flag or exit code, extension hook, or file format users can save | Use the project's stated compatibility policy and supported versions. A released promise counts even if no external caller is visible. Preserve it at its public boundary or follow the authorized breaking-release procedure. |
| A stored row, saved project, delayed job, dead-letter replay, checkpoint, or retained event | Identify which historical values can be read, restored, or replayed. Choose an existing migration path or one boundary reader that accepts the supported history. |
| Producer and consumer can deploy or roll back independently | Check old writer → new reader **and** new writer → old reader. Stage the reader and writer changes in an order supported by the deployment system. |
| Retired contract with evidence of migration and an explicit end of support | Remove the adapter and its retirement-only checks together. Retain tests for the behavior that survives the retirement. |

## Gather evidence proportionate to the risk

Search exports, direct calls and configuration strings. For an apparent rename, inspect serialization keys, CLI registration, package exports and fixtures at the affected boundary. For a real rollout, check the release policy, deployment order, retention, backfill status, and whether rollback can reintroduce an old reader or writer. Use evidence available to the task. A search with zero matches is persuasive for a private lexical symbol and weak for a published API or data at rest. Silence in telemetry is meaningful only if the relevant population, retention and replay paths are covered.

If exposure is still unclear, carry on with work behind the boundary. Preserve the existing public shape if that satisfies the request. If the requested change itself depends on breaking an ambiguous contract, state exactly which artifact or consumer would resolve it and ask the narrow question. Avoid introducing a second name just because a future consumer could exist.

## Keep temporary readers small and removable

Suppose job producers are changing the wire field `limit` to `row_limit`. A retained job contains `{"limit": 0}` and zero is valid. If the old field is observed in retained jobs, teach the queue decoder to normalize it once. Give the rest of the application one typed value. Preserve the distinction between a missing key and zero. Specify the treatment of both keys, an invalid value, and neither key from the actual contract. Do not manufacture aliases such as `max_rows` from similar names in source code. If renaming the internal value meets the request, leaving `limit` on the wire can avoid a wire migration entirely.

For a real wire change, deploy readers that can handle both supported shapes while writers still emit the old one. Enable new writes only after every reader that can receive them, including an allowed rollback target, can handle them. Use the project's existing rollout mechanism. If rollback to an old-only reader must remain possible, keep writes compatible for that window. Avoid scattering dual reads or casually writing both keys when older parsers reject extra fields.

For a temporary decoder, identify an observable exit condition. For example, verify that old writers have stopped, the oldest retryable or replayable job from those writers has expired or been migrated, and eligible rollback versions understand current writes. Include archive restores if the project promises them. Put that condition beside the adapter or in the existing migration tracker. A permanent file-format guarantee legitimately has no sunset. Use a representative old fixture for as long as the guarantee lasts.
