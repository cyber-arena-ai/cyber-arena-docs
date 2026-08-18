# Functionality checks


A defense must be a security change rather than a service degradation. The
functionality check is what separates the two, so its design is a scoring
decision and not an operational health concern.

The check is also the whole of the defender's rulebook. The defender holds
privileged access to its own box and may change the application however it
likes, and the design deliberately does not enumerate permitted edits.
Instead it declares the minimal features that must survive, and any change
preserving them is a legitimate defense regardless of how it works. This
keeps the defensive side open in the same way the offensive side is open: as
an unanticipated genuine attack path scores, an unanticipated genuine
hardening counts.

Checking happens at two levels. A shallow probe establishes that the service
is alive and reachable. A deep checker exercises the declared service
behavior and is the level that decides whether a defense is legitimate. The
deep checker should run outside the defender's editable surface — over the
network from the poller rather than from a script shipped inside the box it
judges — so that a defense cannot rewrite the check that grades it.

The deep checker covers, minimally, the features on every materially
distinct cell-reaching chain known at authoring time
([H2](../invariants.md#h-functional-core)). Minimality is
per-chain: the smallest workflow that exercises that feature's real
semantics, not the smallest workflow overall. A checker covering only the
originally selected attack chain would let a defender amputate the feature
carrying a different genuine scoring path and still pass.

Each run should be randomized. Fresh identities, fresh secrets, and fresh
state per run, with assertions on the workflow's semantic outcome rather
than on fixed strings or fixed paths. A round trip that creates its own
participants and then asserts who may and may not observe the result
distinguishes a working application from a canned imitation; a fixed
request compared against a fixed response does not.

Randomization should also be repeated rather than single-shot. Each run is
recorded independently, and reading a defense as functional only when
repeated randomized runs all pass is one interpretation of that record —
the strictest, and the one that removes the residual chance that a canned or
partially amputated implementation happens to match a single run's shape.
The orchestrator does not apply that rule; it supplies the runs.

The checker is bound by a two-sided constraint. Feature amputation and
canned responses must fail it, and an equivalent narrow hardening must pass
it. A checker that rejects both is coupled to the implementation rather
than to the declared behavior, and fails as a checker even though it looks
strict.

Paths discovered after authoring are not covered by construction. That
residual is accepted; no finite check proves every scoring route is
protected.
