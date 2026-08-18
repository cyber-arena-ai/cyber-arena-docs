# State and measurement

## Statefulness and convergent placement

The application is stateful, it is the thing under attack, and both parties
hold enough access to damage it. Every probe must assume that, because
damage to state inside the box carries no signature: an attacker with
execution and a defender rebuilding an image leave the same evidence, and
attribution is not reliably possible after the fact.

Four assumptions follow, and they are chosen so that most damage needs no
attribution at all.

1. **All evidence is facility-owned.** Canaries and seeded context sit
   inside the box but belong to neither team. Damage to them is a
   measurement problem before it is a game event.
2. **Derivable state is restored, never scored.** The context follows from
   the seed, so the facility re-derives and re-applies it. Nobody is
   charged, because nobody has to be identified.
3. **Non-derivable state is scored.** A canary is fresh randomness and
   cannot be reconstructed once destroyed, so its integrity is measured and
   its loss is charged. This is the only place attribution is needed.
4. **Non-derivable state is therefore kept minimal.** Every precious value
   is a potential dispute; one canary per cell and nothing else.

Availability follows from the same design without extra rules. The
functional check provisions its own participants, so a defender's
availability does not depend on seeded context and an attacker cannot damage
a victim's uptime by destroying it.

One residual is accepted rather than solved. An attacker that destroys a
canary instead of taking it forfeits its own capture, so the behaviour is
self-defeating; what remains is sabotage to deny other attackers, which
matters in a many-team game and is marginal in a benchmark. Charging the
defender there is wrong but rare, and the alternative costs more complexity
than it returns.

### Placement converges

Idempotence is not enough. Running twice without extra harm says nothing
about running against a box whose state has been mangled.

> Placement must be **convergent** ([F2](../invariants.md#f-probes-and-state)):
> run from any prior state — missing, duplicated, corrupted, re-permissioned,
> half-deleted — it lands on the declared state.

This is a desired-state contract. The handler declares what the context
should be and reconciles toward it, rather than assuming what it will find.
Reconciliation runs through the application's own mechanism like every other
placement action, so removing entries that should not exist means using the
application's interface rather than resetting a volume. That is real
authoring work and it is the price of the assumption.

Round ordering becomes part of the contract rather than an implementation
detail:

1. **Observe integrity**, before anything is repaired. Reconciliation that
   runs first silently erases the tampering signal it was meant to detect.
2. **Converge the context**, so the canary has somewhere to live.
3. **Rotate the canary** into the reconciled context.
4. **Arm the observed regions** for the round.

A convergence failure is a defender-fault signal. A healthy application
always accepts the facility's privileged writes, so reconciliation that
cannot complete means the application is broken or its privileged interface
has been amputated. Both are the defender's account rather than an
infrastructure error, which also closes a cheat: a defender that breaks the
administrative interface to prevent planting would otherwise look like a
flaky box.

## Inconclusive measurement

Every probe is three-valued. A capture attempt can succeed, fail, or be
impossible to evaluate; an integrity read can find a canary intact,
damaged, or unreadable; a functional check can pass, fail, or not complete.
The design must say what an inconclusive measurement means, because the
record it feeds is two-valued unless something forces otherwise.

The answer is that the orchestrator instruments rather than judges
([G1](../invariants.md#g-measurement-and-recording)).

> A match produces a log of what was observed, including what could not be
> determined. Scores are computed from that log afterwards, so no orchestrator
> component decides a verdict.

Retry is the only exception, because it is the only response that cannot be
reconstructed later: a claim has to be re-observed while its round is still
live. Everything else — whether an unreadable canary counts as tampering,
whether an unreachable box costs availability, how a blocked facility
channel is treated — is a reading of the log rather than a decision taken
during the match.

Availability is therefore telemetry rather than a penalty. The poller
samples, the samples are recorded with their outcomes, and any definition of
uptime can be computed from them later. A defender that keeps the service
running but blocks the facility's privileged channel is recorded as its own
event kind rather than being priced during the match, and the same holds for a
convergence failure that cannot be separated from an infrastructure fault
in the moment.

This moves the burden from deciding correctly to **recording sufficiently**.
A log entry reading `not captured` where the truth was `box unreachable`
destroys a distinction no later analysis can recover, so every record
carries its reason:

- the cell, the attacker, the victim, the round, and the time.
- the outcome, and the reason behind it — reached, not reached, not
  reachable any longer, or not measurable.
- for an inconclusive result, which probe could not decide and what
  prevented it.
- for a retried claim, how many attempts were made.
- for a repaired context, that repair occurred and what it replaced.

Distinctions the record must preserve because scoring may want either
reading: a cell reached this round against one retained from an earlier
round, a cell never reached against one a defender legitimately hardened out
of existence, and a defender's damage to a canary against a box that could
not be read.

The log lives with the facility, is append-only, and is not derivable from
anything inside the box, for the same reason
[capture evidence is not](../authoring/boundaries.md#genuine-and-arena-specific-paths).
