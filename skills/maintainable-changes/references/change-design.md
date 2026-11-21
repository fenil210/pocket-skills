# Design for the next edit

## Find the owner of the behavior

Start at an entry point someone uses. Follow one input to the decision and one result back to the caller. Search for a distinctive field name, error, unit, or formula as well as the function name. A second implementation often has a different name. Identify which module has enough information to make the decision and which callers need the answer.

Suppose an importer accepts 100 rows and the HTTP handler, worker, and preview UI each check `100`. Find out what each limit means. If preview and import enforce the same product allowance, put that rule in the existing policy owner and let its callers consume it. If the worker batches at 100 because of a database parameter limit, keep that independent. A shared constant named `MAX_ROWS` would let a product change quietly alter database behavior.

## Compare the cost of two plausible patches

For a nontrivial choice, walk one realistic follow-up through each patch. Ground it in a neighboring feature, an already supported input, a bug report, or a rollout the project actually performs. Count decisions that must agree, places that can drift, and existing callers forced to learn a new convention. A slightly larger patch that removes the second copy of a policy can make the follow-up smaller. Stop once the tradeoff is clear. A plausible follow-up is a way to choose today's design, never a new feature request.

| Decision in the diff | What would justify keeping it? | First thing to try |
| --- | --- | --- |
| Pass-through wrapper | It adapts units, normalizes a provider response, owns a transaction, or shields an independently changing contract. | Call the existing operation directly when both interfaces say the same thing. |
| New interface or base class | Existing implementations vary on a real axis, or a real I/O boundary needs isolation. | Use the existing boundary or a function. Check how a second caller actually differs before making it configurable. |
| Flag or setting | A named operator or caller chooses between supported behaviors, or a rollout needs an off switch. | Choose one implementation for a settled requirement. For a temporary switch, identify when it can be removed. |
| Extra error branch | The boundary can supply this input or failure, and the caller has a defined recovery. | Validate once at ingress. Use the existing error vocabulary and preserve information the caller acts on. |
| Generic helper | The callers share meaning and change together, or one cohesive operation needs a name to be readable. | Keep independent rules local when their similarity is incidental. A small single-use helper can still clarify units or a nontrivial calculation. |

## Follow the interruption point

When a change mutates durable state or crosses a process boundary, pause at the first side effect. What happens if the next line never runs, the request is retried, or two workers arrive together? Inspect existing transactions, uniqueness constraints, retry ownership, and recovery paths before adding machinery. For example, if a row write and checkpoint share one database, see whether the existing transaction can commit them together. For a remote side effect, look for an existing stable request ID recognized by the recipient. A flag set in memory cannot protect a later process after restart. Preserve error propagation where callers already own retry decisions.

When a change is a pure local calculation, keep the design local. A registry, queue, or retry policy needs a present job to do. Finish the patch at the point where the requested behavior and the established contract are both satisfied.
