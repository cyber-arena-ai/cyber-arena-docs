# Access layers


The model is concrete once the running challenge is read as three access
layers, because each participant enters at exactly one of them and each
layer carries a single rule.

| Layer | Who acts there | Rule |
|---|---|---|
| **Public surface** | the attacker starts here | public surface; holds no secrets and assumes no good faith |
| **Guarded surface** | the facility acts here by right; the attacker must arrive here | typed operations whose protection the application authors — objectives live here |
| **Infrastructure** | orchestration: container, filesystem, SSH, network | neither a scoring surface nor objective material |

## What each layer contains

### Public surface

Everything the application offers to the baseline identity. A baseline
session is part of the public surface rather than past it: the attacker's
legitimate reach is the starting position, not a first success.

- The service ports declared public, and the protocols served on them.
- Every endpoint, view, and static asset reachable on those ports without
  authentication.
- The authentication surface itself: login forms, token issuance, session
  handling, and registration where the application enables it.
- The baseline identity — its credentials, its session, and every operation
  the application permits it to perform.
- Application data the application publishes to that identity, including
  anonymous-visible records.
- Banners, version strings, error text, and any other output the
  application emits to unprivileged callers.
- Whatever the challenge hands the attacker at start: the service address,
  the target list, the published objective names, and the baseline credentials.

*Test: reachable using only the rights the baseline identity already holds.*

### Guarded surface

Everything the application exposes but gates behind authority it enforces
itself. Protection the application established and the operating system
enforces belongs here, because establishment rather than enforcement
mechanism is what makes a boundary the application's own.

- Administrative and owner-only operations: user and role management,
  system configuration, extension or plugin installation.
- Operations on another principal's records, whether scoped by tenant,
  owner, project, or namespace.
- The credential or secret store, and the typed operation the application
  exposes for adding and reading entries in it.
- Configuration the service itself acts upon: job and pipeline definitions,
  workflows, scheduled tasks, hooks.
- Execution as the service account, including any feature whose declared
  purpose is running commands or scripts.
- The application's at-rest representation of all of the above, as the
  application writes it — its configuration files, database rows, and
  encrypted stores.
- The authorization logic guarding them: permission model, roles, access
  control lists, ownership checks.
- Protection the application established for itself: file modes and
  ownership it sets, keys it generates, directories it restricts.
- Endpoints the application exposes only to authenticated or privileged
  principals, including internal and loopback-only surfaces.
- Seed data — identities, records, jobs, credential entries — where the flag
  handler created it through an application operation.

*Test: the application refuses it to the baseline identity, by logic or by
protection the application itself established.*

### Infrastructure

Everything present in every challenge regardless of which application is
deployed.

- The container, its image wrapper, its entrypoint, and its lifecycle:
  start, stop, rebuild, restart.
- Privileged shell access to the box, and the transport carrying it.
- The arena network, inter-team routing, and the isolation between boxes.
- The gameserver: round scheduling, scoring, the canary service, mark
  issuance, and the ledger of claims.
- Agent-facing tooling and the tool surface it is driven through.
- The probes themselves, and the facility setup scripts that install them.
- The defense edit path: the writable source tree exposed to the defender
  and the restart action that applies edits.
- Orchestrator state: round identifiers, unissued marks, canary values held
  server-side, and the handles locating them.
- Challenge metadata: the registry entry, the documentation, the declared
  attack scopes.
- Poll intervals, timeouts, resource limits, and availability accounting.
- Any host path, file mode, or ownership the challenge created rather than
  the application.

*Test: it would still be there if a different application were deployed.*

The infrastructure test is the practical one, and it settles the case that
otherwise looks ambiguous: a directory mode is infrastructure when the
challenge set it and a privileged action when the application set it,
because only the second survives replacing the application.

The infrastructure rule cuts symmetrically, and that symmetry is the point.
The attacker may not score through challenge infrastructure, and the author
may not build an objective out of it. Infrastructure is the stage rather than the
play.

An objective is then the gap between what the facility holds by right and what the
attacker starts with. That gap is asymmetric by construction, and the
asymmetry is the challenge.

## Who supplies the data

An application in this arena is a runtime plus data, and the data has three
suppliers whose separation determines whether an objective is sound.

| Supplied | By | Rotates |
|---|---|---|
| Runtime and the default protection of each asset class | upstream image | no |
| Application state — identities, records, jobs, credential entries | the challenge author, at provisioning | no |
| Canaries and marks | the orchestrator, per round | yes |

Author-supplied state is not decoration: it is what makes an asset instance
exist at all. It is also the layer where objectives are most easily invalidated,
because the author who can create an asset cannot create its protection.

The orchestrator's contribution is separate in kind. A canary or mark is a
**canary** measuring an objective rather than an asset, which is why it rotates
while the asset does not. The test is deletion: remove the canary and the
credential store is still a protected store that can no longer be measured;
remove the seeded credential entry and there is nothing there to protect.

## Planting goes through the application's own mechanism

The facility is not a black-box client. It is the **privileged principal** —
it plants the credential as the administrator who owns the credential store,
and it observes the mark as root on the victim's box. A facility that could
plant without authority would describe an objective the attacker already holds.

The constraint is therefore on how it plants rather than on how much
authority it has ([E1](../invariants.md#e-facility-conduct)):

> The facility plants through the operation the application exposes for that
> asset class, at the privilege that operation requires.

Adding a credential through the application's credential API and writing the
same bytes into its secrets directory as root reach the same on-disk state,
but only the first is an objective. In the second, the path, the mode, and the
protection were all chosen by the challenge, so the attacker's target is the
challenge's file placement rather than the application's credential
subsystem.

One question decides it ([E2](../invariants.md#e-facility-conduct)): **after planting,
did the challenge have to add any protection itself?** If it chmods, chowns,
or picks the directory, the challenge is the mechanism. If it hands the
application a typed object and the application stores it where it stores
those, the application is.

The line is application code against challenge code, not remote against
local. An initialization script running inside the service and calling the
application's own object model is application mechanism; a request to an
endpoint that writes a file the challenge named is not.

An asset with no such operation is not a packaging problem to work around.
It is evidence that the application does not treat that asset as protected,
and the objective does not exist.

## Plant at layer two, observe from layer three, attack from layer one

Observation is the one deliberate asymmetry. The observer reads at the
infrastructure layer — over SSH, off the victim's disk — and that is sound
because planting *creates* the asset while observation only *measures* a
state the attacker already caused. Observation carries a different
constraint instead: it must read the application's authoritative
representation of that state rather than a shadow copy the challenge
maintains.
