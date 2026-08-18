# Probes and hooks


Probes should be organized by what they do to the running box and whom they
measure, because that is what determines the rules each one must obey.
Grouping them by when the orchestrator happens to call them puts unlike
things together and separates like ones.

| Probe role | Measures | Rule it must obey |
|---|---|---|
| **Placement** | nothing; it instruments an objective | acts as the facility through the application's own mechanism, and is idempotent under retry |
| **Capture** | the attacker | proves one objective was reached, from a vantage the attacker cannot forge or replay |
| **Integrity** | the defender | reports whether a canary still stands, and distinguishes damage from an inconclusive read |
| **Functional** | the defender | exercises the declared functional core without encoding an attack path |
| **Restart** | nothing; it is an action | rebuilds and restarts on the defender's request, and reports honestly whether the service came back |

Two separations in this table are deliberate and should be preserved even
when one implementation happens to serve both sides.

The **read and observed oracles are distinct placement-and-capture pairs**,
not two modes of one mechanism. A read objective is instrumented by planting and
captured by submission; an observed objective is instrumented by arming a region
and captured by privileged-side observation of a per-attacker mark. They
share a result vocabulary so scoring treats them alike, and nothing else.

**Capture and integrity face opposite parties.** Capture asks whether the
attacker reached an objective. Integrity asks whether the defender damaged the
canary in it. A single read of a planted value can answer both questions, but
they must remain separately expressible: a defender who deletes a canary and
an attacker who never found one produce the same missing value and must not
produce the same verdict.

Infrastructure is excluded as objective material and as a scoring surface. It was
never excluded as a tool, and a blanket rule that probes must stay at the
application interface would forbid the restart role outright. The rule that
holds instead is that **the layer a probe uses must match the party it
measures**.

| Probe role | Layer it may use | Why |
|---|---|---|
| Placement | the application's own mechanism only | it decides what protects the canary; reaching past the application means the challenge authored that protection |
| Capture | infrastructure, preferentially | it measures the attacker, so the vantage must be one neither party can control |
| Integrity | the application as privileged principal, infrastructure as fallback | it measures defender damage, and reading at rest risks reading a representation the application does not consider authoritative |
| Functional | the application's user-facing interface only | it measures whether the service still serves users |
| Restart | infrastructure | it is an action on the box rather than a measurement |

The two ends of that table invert, which is the part worth stating plainly.
Placement must stay inside the application because it establishes protection.
Capture should leave the application precisely because the application
surface is defender-controlled: an observed oracle that asked the service
whether a mark had landed would let a defender answer on its behalf. Where
capture reads at the infrastructure layer, it must read the representation
the application itself treats as authoritative rather than a copy the
challenge maintains.

The functional role carries the strictest form of the rule, and it is the
one most often relaxed for convenience. Any shortcut to a privileged
vantage — reading state at rest, consulting service internals — produces a
check that an amputated service still passes.

A probe cannot be required to leave the application untouched, because a
functional probe must exercise it, and exercising a real application changes
state. The rule is narrower ([F1](../invariants.md#f-probes-and-state)):

> A probe may change transient state. It may not create an asset or add
> protection to one.

Durable state that a later round depends on therefore belongs to deployment,
never to a probe. A restart path that writes a protected baseline for a
later functional comparison has crossed this line, even though the value it
writes is derived from the application. Deployment hooks — initial start,
seeding, deliberate breakage, facility setup — run at bring-up and are
allowed to create state precisely because they run before the contest does.
