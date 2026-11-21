---
name: maintainable-changes
description: Implement, refactor, or review code with long-term maintainability in mind. Use for feature work, bug fixes, pull request review, or pruning agent-generated code when tests, comments, abstractions, or backwards compatibility need judgment. Trace actual callers and stored data, protect meaningful behavior, and remove complexity a later change would otherwise have to carry.
---

# Maintainable Changes

Make the requested change easy for the next person to revise. Judge each addition by the behavior it protects, the information it preserves, or the decision it lets a future maintainer make in one place. Use repository conventions and the user's scope. A review request produces findings. An implementation request produces a working patch.

## Work from the code outward

1. **Trace the contract.** Start with the normal entry point and its nearest tests. Follow the changed value to its callers, output or storage. Identify who owns the rule, who relies on it, and which part the request changes. For a local edit, stop tracing when the boundary is clear.
2. **Walk one likely follow-up.** For a meaningful design choice, pick a later edit suggested by this codebase. Where would someone change the rule? Which places would need to agree? Prefer a design with one owner and few coordinated edits. Implement only today's requirement. Use plain control flow and the project's existing seams.
3. **Challenge what the patch adds.** For a helper or flag, name the decision it owns. For a compatibility path, name the supported caller, saved data, or rollout that needs it. For a comment, name the fact a reader cannot infer. For a test, name the plausible defect and observable failure. Remove an addition whose only purpose disappears under that question. Apply the same scrutiny before deleting an existing guard or test.
4. **Verify through the affected boundary.** Use the closest checks that exercise the delivered behavior. For a bug, establish where the previous behavior would fail. For public or persistent changes, exercise applicable historical data and rollout directions. For side effects, consider interruption, retries and concurrency where those are possible. Report what actually ran and material gaps.

## Use the reference that matches the decision

| If the patch involves… | Consult |
| --- | --- |
| A wrapper, duplicated policy, new flag, or a change in failure handling | [Design for the next edit](references/change-design.md) |
| A rename, fallback, versioned payload, migration, public interface, or rollback | [Decide who is owed compatibility](references/compatibility.md) |
| Adding, updating, or pruning tests, including mocks and snapshots | [Choose tests a maintainer can trust](references/tests.md) |
| Adding or removing comments, docstrings, workarounds, or directives | [Give comments a job](references/comments.md) |
| A diff that passes its tests yet fails between two side effects | [Worked review of an export restart](references/worked-review.md) |

## Finish at the user's scope

Reread the diff as the person making the next edit. Remove scaffolding introduced for possibilities this change never needed. Keep unrelated cleanup out of the patch. Prefer an existing formatter, type check or lint rule for a mechanical convention. Add harness rules only when requested or when the task explicitly requires one and the rule has a reliable signal. Never use comment counts, test counts or patch size as quality targets.

For implementation, summarize the behavior and verification. Mention a compatibility or design tradeoff only if it changes what the user should know. For review, lead with concrete findings, ordered by impact. Give a location, the triggering condition, the consequence and the smallest adequate correction. If the change holds up, say so. Keep the working checklist private unless requested. Document lasting contracts where the project already keeps them.
