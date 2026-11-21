# Choose tests a maintainer can trust

## Name a failure before writing the assertion

Complete the thought “If someone changes ___, a caller could observe ___, and this assertion would fail.” Find the nearest existing test of that outcome. Read its setup and assertion. A name containing `retry` tells you very little about which retry it exercises.

- **Reuse** an existing test when it already fails for the plausible defect. A private rename that preserves behavior usually ends here.
- **Extend** an existing test when the same setup needs a missing input or branch. For a new rule that treats a zero limit differently from a missing limit, one focused case in the existing table may suffice.
- **Add** a test when a real behavior, regression, or required contract has no reliable signal. Choose the least expensive boundary that can actually fail in the relevant way. Honor repository-required checks.
- **Replace or prune** a test within the requested scope when a surviving check catches the same failure through an appropriate boundary. Check its fixtures, historical regression, configuration and failure mode first. Preserve a unique guarantee by moving its decisive assertion before deleting the old test.

## Choose what to observe

| Change | Weak signal | Signal that would catch the defect |
| --- | --- | --- |
| Retry after downstream timeout | Assert a private `retry()` helper was called twice. | Give the fake recipient a timeout after accepting the first request. Assert the retry uses the same operation ID and the recipient records one effect. |
| Persisted filter | Construct an object, then assert the constructor returns it. | Save a distinctive value, reload through the normal reader, and assert the value survives. Use a historical record when old data is the risk. |
| Batch lookup with a strict I/O budget | Compare every internal method call in sequence. | Measure calls across the actual storage boundary if a single round trip is a promised constraint. Allow internal reordering that preserves that constraint. |
| Output document or stable wire format | Approve a full snapshot containing timestamps and random IDs. | Assert the contractual fields or use a small, reviewable golden file where the complete rendering or exact bytes are the product. |

Use the real pure logic. Fake the clock, filesystem, queue or network at the seam that introduces nondeterminism. For database constraints, real serialization, or migrations, use the project's existing integration harness when an in-memory fake would assume the very property at issue. A fast unit test for a calculation and an integration test for its deployed wiring can both have value. Similar inputs alone do not make tests duplicates.

## Check that the test can fail for the right reason

For a regression, run the focused test on the failing behavior when practical, then on the fix. If the starting tree already includes the fix, use a disposable copy or reason through one precise fault, such as replacing `>=` with `>` at the inclusive boundary. Check that the relevant assertion catches it. Keep user edits intact. Distinguish a witnessed failure from a reasoned one when reporting results.

A test that still passes after removing the behavior often asserts its own fixture, mocks the function under test, or accepts both the correct and incorrect outcomes. A test that fails after a private rename often binds to a detail callers cannot observe. Repair the observation. For flaky concurrency, synchronize on a state transition or an existing test hook so the ordering is reproducible. Sleeping longer or widening assertions can conceal the defect. When a test fails after a refactor, determine whether the contract changed before updating the expected value.

Stop once the changed behavior and material risks are covered. File counts, coverage deltas and the number of assertions alone do not tell you what will break. Run the project's relevant checks and report any specific gap the environment leaves.
