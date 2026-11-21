# Worked review · a missing export row

## Read the report against the code

An export occasionally skips a row after a worker restarts. The runner processes rows serially. Its stored `next_row` is the first row a restarted worker will read. The storage client returns after a durable write and treats repeated writes of the same `(export_id, row.number)` and contents as a single result. The worker retries a failed run. Those facts are inputs to this example. In a real repository, find their equivalents in the runner, storage contract and recovery path.

```python
def _deliver(export_id, row, sink, progress):
    progress.set_next(export_id, row.number + 1)
    sink.put((export_id, row.number), row.data)
```

If `sink.put` fails before storing the row, the checkpoint already points past it. A proposed fix adds a `ReliableSink` wrapper, a private alias named `_deliver_legacy`, and a mock test that requires calls to both objects. The test passes on the code above. Both calls occur on the happy path with either ordering.

## Make the failure impossible under the stated contract

```python
def _deliver(export_id, row, sink, progress):
    # Restart resumes at the checkpoint. Replaying this storage key is safe.
    sink.put((export_id, row.number), row.data)
    progress.set_next(export_id, row.number + 1)
```

Now a failed write leaves the checkpoint on the unconfirmed row. If the write succeeds and checkpointing fails, the retry uses the same storage key. The existing worker owns retries. There is one recovery decision and no new interface. The caller still supplies the same arguments and existing checkpoints keep their meaning. An alias would solve no compatibility problem here.

Inspect the existing restart test. If it only restarts between completed rows, extend it or add a focused case through the runner. Start at row 12, make storage fail once for that row, and retry **from the persisted checkpoint** after reconstructing the worker. Verify row 12 reaches storage and the checkpoint advances on success. Rewinding by hand inside the test would make the broken version pass. If an existing test already covers the gap, use it. Cover the opposite interruption point separately if sink deduplication or checkpoint retries also changed and lack coverage.

## Walk one later change through the patch

A likely follow-up batches ten rows per storage request. A test that insists on one `put` per row would fail even when recovery works. The restart test still expresses the intended outcome. A new design must advance the checkpoint only through the **contiguous run of confirmed rows**. A batch that can partially succeed or a runner that processes rows concurrently needs evidence about which rows became durable. The simple serial fix depends on that distinction. If the sink can append the same record twice, reversing two lines cannot establish exactly-once output.

A concise review finding for the original diff could say “`export.py`, `_deliver` — If storage fails on row 12, the saved checkpoint already says 13. The next run skips row 12. Move the checkpoint after the confirmed write and exercise the restart using the saved checkpoint.” The wrapper's naming would add little to that review.
