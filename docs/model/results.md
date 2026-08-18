# Reproduction and escalation


The selected vulnerability's impact identifies which cells its floor
witness reaches. Those cells are labeled reproduction for that instance,
and every other captured cell is escalation. The label is per-instance
metadata rather than a property of the cell or its scope, so the same scope
is reproduction in one challenge and escalation in another.

Results are recorded as raw per-cell capability records — which cell, which
attacker, which victim, which round — rather than as a score. Aggregation
weighting, per-round versus per-match credit, and any interaction with
availability are left open deliberately, because a raw record can be turned
into any of those later while a score cannot be turned back.

Reproduction and escalation are therefore a reading of the record relative
to the floor, computed after the fact. That keeps outcomes
comparable across applications that expose different reachable subsets, and
it keeps the cell set itself free of any assumption about which path an
agent takes.
