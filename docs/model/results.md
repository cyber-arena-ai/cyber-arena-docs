# Reproduction and escalation


The selected vulnerability's impact identifies which objectives its floor
witness reaches. Those objectives are labeled reproduction for that instance,
and every other captured objective is escalation. The label is per-instance
metadata rather than a property of the objective or its scope
([H2](../invariants.md#h-interpretation)), so the same scope is
reproduction in one challenge and escalation in another.

Results are recorded as raw per-objective capability records — which objective, which
attacker, which victim, which round — rather than as a score. Aggregation
weighting, per-round versus per-match credit, and any interaction with
availability are left open deliberately, because a raw record can be turned
into any of those later while a score cannot be turned back.

Reproduction and escalation are therefore a reading of the record relative
to the floor, computed after the fact. That keeps outcomes
comparable across applications that expose different reachable subsets, and
it keeps the objective set itself free of any assumption about which path an
agent takes.
