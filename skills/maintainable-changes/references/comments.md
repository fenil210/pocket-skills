# Give comments a job

Read the next few lines as someone about to modify them. Identify the attractive edit that would be wrong and the missing fact that makes it wrong. A comment earns its place when it gives that fact to the person at the edit site. If the problem is simply that the code takes three passes to understand, first try a better name, a unit-bearing type, or a simpler branch.

| What the text contains | Where it belongs |
| --- | --- |
| Units, a domain term, or an ownership relationship a name can express | Encode it in a name such as `retry_delay_ms` or `lease_owner`. Update the related uses. |
| A constraint imposed by another system, a subtle ordering requirement, or why a tempting simplification loses data | Keep a short rationale next to the decision. Include enough of the fact to stand on its own if a linked issue disappears. |
| A promise callers need before they use an API | Put accepted values, side effects, or error semantics in the established public docstring or API docs. Implementation notes do little for callers. |
| License or attribution text, generated-file markers, type directives, lint suppressions, or protocol annotations | Treat these as inputs to legal, build, or tooling workflows. Preserve required text and investigate the consumer before editing it. |
| A retelling of syntax, patch history, a self-congratulatory note, or a divider between two obvious calls | Remove it from touched code. Let the diff or history record the edit. |

Suppose the export protocol resumes **inclusively** at the saved offset after a crash. That fact changes which offset is safe to store.

```python
# Resume starts at the saved offset. An early checkpoint would skip an unflushed row.
writer.flush()
checkpoint.save(next_unwritten_offset)
```

This comment helps someone considering swapping the calls. The line `# Flush the writer` would merely echo the method. Check the project's actual durability guarantee before writing either assertion about the protocol. If the flush and checkpoint can fail independently, a comment cannot make the operation atomic. Handle duplicate delivery if the contract requires it.

When shrinking a complicated expression, try moving the explanation into the representation. `expires_at <= now` with typed instants is easier to review than a comparison of anonymous integers plus a paragraph explaining their units. Keep a short comment if the inclusive boundary has an external reason, such as a documented provider cutoff.

A temporary workaround deserves the condition that makes removal safe. “Remove when the last worker that reads format 1 is outside the rollback window” gives a future maintainer something to verify. Tie it to an existing tracker if one exists. Avoid vague promises such as “simplify later” and invented issue numbers. Preserve an older comment when its reason still applies. If it conflicts with the code, check the underlying contract before correcting either one.
