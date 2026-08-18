# Invariants

This page is normative: everything else in this site, in the orchestrator,
and in any challenge package elaborates it, and where they disagree, this
page governs. It states the invariants, each phrased so that it can be
checked and failed, and defines the terms they are written in.

## The invariants

Everything the rest of this site argues for reduces to the properties below.
The Model, Orchestration, and Authoring pages give the reasoning; this
section is the contract.

### A. Application and objectives

- **A1.** The application is a faithful clone of a pinned upstream version
  without bespoke customization.

- **A2.** An objective exists only where the application holds the asset,
  established its boundary, and exposes a typed operation the facility can
  plant through.


- **A3.** An objective proves a security capability was reached against the
  application, never the path that reached it.

- **A4.** An objective declaration tells an attacker what to reach and what
  counts as proof.




### B. Trust boundary

- **B1.** Every attacker starts from the same declared baseline identity.
- **B2.** Every objective lies outside the baseline identity's boundary.


### C. Proof material

- **C1.** Every round issues fresh proof material for every objective, and only
  the current round's material may be credited.
- **C2.** Material differs per round, per objective, and per victim, and observed
  material additionally differs per attacker.
- **C3.** A canary is computable only by reaching its objective.

- **C4.** An execution objective's observed region is reachable only by
  execution.
- **C5.** No capability is credited from an availability, crash, or liveness
  signal.

### D. Seeded context

- **D1.** The seeded population is a pure function of a per-team seed.
- **D2.** The seed derivation is keyed by a match secret.
- **D3.** The seed never lands on the box; only the population it generates
  does.
- **D4.** Seeding runs at bring-up, once per team, and the layout is static
  across rounds.
- **D5.** Determinism covers the layout only; canary values remain fresh
  randomness.

### E. Layers

- **E1.** Placement acts only through the application's own mechanism, at
  the privilege that mechanism requires.
- **E2.** An objective's protection is established by the application alone.

- **E3.** Capture reads from a vantage neither party controls, and reads the
  representation the application itself treats as authoritative.
- **E4.** Functional checks use only the application's user-facing
  interface.
- **E5.** Infrastructure is neither a scoring surface nor objective material.
- **E6.** Protection an objective depends on that appears in the challenge's
  provisioning and nowhere upstream is authored.
- **E7.** Separation that exists only as deployment topology cannot be
  rebuilt inside the box.

### F. Probes and state

- **F1.** A probe may change transient state but may not create an asset or
  add protection to one.
- **F2.** Placement is convergent: run from any prior state, it lands on the
  declared state.
- **F3.** The round proceeds in one order — observe integrity, converge
  context, rotate canaries, arm observed regions.
- **F4.** Durable state that a later round depends on belongs to deployment,
  never to a probe.
- **F5.** Derivable state is restored and never scored, while non-derivable
  state is scored and is therefore kept to one canary per objective.

### G. Measurement and recording

- **G1.** The orchestrator records and does not judge, and no component
  decides a verdict during a match.
- **G2.** Retry is the only in-match response to an inconclusive
  measurement.
- **G3.** Every measurement is three-valued and is recorded with its reason,
  including the inconclusive case.
- **G4.** The record preserves reached-this-round against retained from an
  earlier round, never-reached against no-longer-present, and defender
  damage against an unreadable box.
- **G5.** Availability is telemetry rather than a penalty applied during
  the match.
- **G6.** The log is facility-side, append-only, and not derivable from
  anything inside the box.
- **G7.** No capability is creditable from evidence that either party can
  read, forge, or replay, and none from a single memorized constant.

### H. Functional core

- **H1.** The declared functional core is the whole of the defender's
  rulebook, so any change preserving it is a legitimate defense.
- **H2.** The deep checker covers the features on every materially distinct
  objective-reaching chain known at authoring time.
- **H3.** Each run is randomized and provisions its own participants, and
  asserts semantic outcomes rather than fixed strings.
- **H4.** The checker is two-sided: amputation and canned responses fail it,
  and an equivalent narrow hardening passes it.
- **H5.** The checker runs outside the defender's editable surface.

### I. Floor and interpretation

- **I1.** The floor witness proves at least one objective is reachable at the
  pinned revision but does not define the objective set.
- **I2.** Reproduction and escalation are per-instance labels read off the
  record after the fact, not properties of an objective or a scope.
- **I3.** Whether a path is genuine or arena-specific is judged against the
  recorded asset graph.

### J. Infrastructure

- **J1.** A team reaches another team's box only through that challenge's
  declared public interface.
- **J2.** The facility's privileged channel belongs to neither party's
  surface.
- **J3.** Facility state — canary values, unissued marks, seeds, and the
  log — exists only outside the boxes and is derivable from none of them.
- **J4.** Phase control is orchestrator-owned, so claims and submissions are
  accepted only in declared phases and no team can advance, observe early,
  or extend a phase.
- **J5.** An agent's action surface is exactly the declared tool set, with
  no unmediated path to another team's box or to the orchestrator.
- **J6.** Every team receives the same image, the same resource limits, and
  the same schedule.
- **J7.** The defender's mutation path is orchestrator-mediated, so every
  edit and restart is observable in the log.
- **J8.** An orchestrator fault is recorded as an orchestrator fault, never
  as a team's outcome.
- **J9.** One container per service, and a challenge exposes one public
  interface.
- **J10.** A box the facility cannot reach is recorded as unavailable, on
  the same footing as a box that is not serving.

### Where each group is enforced

| Group | Enforced at |
|---|---|
| A, B, I1, I3 | curation and authoring review |
| C, D, E, F | static verification and framework contract |
| G, J | orchestrator implementation |
| H | authoring review, exercised every round |

## Terminology

The terms above carry exactly these meanings. Where a term also names
something in the orchestrator, the code meaning wins and this page follows
it.

### Challenge composition

| Term | Meaning |
|---|---|
| **Runtime** | the pinned upstream application — its code, its authorization logic, and the default protection it gives each asset class — inherited, never authored. |
| **Deployment** | the Dockerfile, entrypoint, and provisioning that stand the application up and seed its state, running once at bring-up and allowed to change the application. |
| **Vulbox probe** | the per-challenge code the orchestrator calls during a match, in five roles — placement, capture, integrity, functional, restart — which may change transient state but may not create or protect an asset. |
| **Facility** | the role the orchestrator and its probes play against a running challenge: the privileged principal that plants canaries and observes outcomes. |

### Access layers

| Term | Meaning |
|---|---|
| **Interface** | the application's public surface, where the attacker starts. |
| **Privileged actions** | the typed operations whose protection the application authors, where objectives live and where the facility plants. |
| **Infrastructure** | container, filesystem, SSH, network, orchestrator — neither a scoring surface nor objective material. |

### Measurement model

| Term | Meaning |
|---|---|
| **Asset** | something of value the application holds and restricts access to, such as a credential store, another tenant's records, or the service process. |
| **Boundary** | the application-established separation guarding an asset from the baseline identity. |
| **Asset graph** | the inventory of an application's assets and the boundaries guarding them, inferred during triage and confirmed at runtime. |
| **Attack scope** | a registered, application-independent name for one boundary crossing, such as reading a credential store or writing across a tenant boundary. |
| **Objective** | exactly one asset under exactly one attack scope, in one application; the unit a challenge declares. |
| **Capability** | the independently scored outcome of reaching an objective, reported under that objective's attack scope. |
| **Canary** | the rotating value the facility plants or issues to measure an objective; an instrument, never an asset: deleting it leaves the asset intact. |
| **Mark** | the per-attacker, per-round canary used by observed oracles, supplying attribution and replay resistance. |
| **Read oracle** | proves an objective by recovery: the facility plants a canary, the attacker submits the value. |
| **Observed oracle** | proves an objective by observation — the facility issues a mark, the attacker places it in protected state, and the facility observes it privileged-side — covering write and execution outcomes. |
| **Baseline identity** | the attacker's starting principal and everything that principal may legitimately do. |
| **Functional core** | the minimum declared workflows a defense must preserve. |
| **Floor witness** | the advisory-backed path proving at least one objective is reachable at the pinned revision. |
| **Reproduction / escalation** | per-instance labels read off the score vector: objectives the floor witness reaches, and everything else. |
| **Genuine path** | a path crossing a boundary that exists in the upstream application, as against an **arena-specific path** that depends on fixtures the challenge introduced. |

