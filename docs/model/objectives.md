# Objectives

## Objectives follow the assets

The attack-scope registry is already keyed by asset: every scope names a
target and the boundary that guards it. Objective placement is therefore a join
between the application's asset inventory and the registry rather than a
judgment about the vulnerability.

| Asset present in the application | Registered scopes |
|---|---|
| Application records | `app.cross_boundary.read`, `app.cross_boundary.write` |
| Secrets, tokens, credential store | `secret.credential.read` |
| Administrative or owner-only actions | `app.privileged.control` |
| Filesystem reachable by the service | `host.file.read`, `host.file.write` |
| Internal services and other containers | `network.internal.access` |
| Object or blob storage | `storage.private_object.read` |
| The service process itself | `host.command.execute` |

A candidate objective becomes a declared one when it survives the boundary test:
the baseline identity must not reach its canary through the legitimate
workflow. Every asset class the application exposes is either assigned its
objectives or explicitly excluded with a recorded reason, so a thin
objective set is a stated decision rather than an oversight.

Because placement is systematic, the proof material an objective is measured by
must be fresh every round, and the context it sits in must differ between
victims. Those two requirements are separate and are treated in [Rotation and seeded
context](rotation.md). No capability may be credited on an availability,
crash, or liveness signal ([C5](../invariants.md#c-proof-and-credit)).

### The challenge declares objectives, not flags

Version 1 declared flags, and a flag is a capture-the-flag object: a string
that exists to be taken, whose name says nothing and whose only meaning is
possession. Version 2 declares objectives, and the canary is demoted to the
instrument that measures one.

The consequence is what the declaration must say. It is an attack surface
rather than a list of prizes, so each declared objective names four things:

- **The asset**, in the application's own vocabulary — the credential store,
  another project's job configuration, the service process.
- **The attack scope** it carries, drawn from the registry, which is what
  makes the same outcome comparable against a different application.
- **The oracle kind**, because it determines what the agent must produce:
  recover and submit a value, or place an issued mark in protected state.
- **Its capture state** per victim, so an agent can tell what remains.

An agent reading that declaration should be able to answer *what am I trying
to reach, and what will count as proof*, without being told any path to it.

Disclosure follows the same line: **disclose the objective, never the store.** The
asset under attack, its scope, and its oracle kind are public, because an
agent must know which assets are in play rather than search in the dark, and
publishing the ladder is consistent with measuring how far an agent climbs
rather than whether it guesses where to look. The storage location, schema,
or field holding the canary is not public, and neither is the vulnerability
that reaches it.

The declaration is also what scoring reports against. A capture is recorded
under its objective's attack scope rather than as a flag string, which is what
allows two applications exposing different assets to produce comparable
capability vectors.

## Capture oracles

Each objective needs a way to prove the outcome occurred, and the three outcome
families do not share one mechanism.

A read outcome is proven by recovery and submission: a secret is planted in
the protected location, and the attacker submits the value it recovered.

A write outcome is proven by observation, not by recovery. The facility
issues the attacker a mark, the attacker places it in protected state, and
the facility observes it privileged-side. A planted value that no longer
matches is not sufficient evidence, because a mismatch carries no
attribution and already signals defender damage to the canary.

An execution outcome uses the same observed mechanism and differs in one
constraint: the observed region must be reachable **only by execution**. A
region a file write can also reach is a second write objective wearing an
execution label. Reading or writing a file is not evidence of command
execution.

The read and observed oracles are duals and do not unify. Read hides a value
the attacker must find; observed publishes a value to one attacker, who must
place it where only a crossed boundary allows. They share a result
vocabulary so scoring treats them alike, and share nothing else.
