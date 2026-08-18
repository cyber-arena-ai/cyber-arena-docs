# Invariants

This page is normative: everything else in this site, in the orchestrator,
and in any challenge package elaborates it, and where they disagree, this
page governs. It holds the terms, used with exactly one meaning throughout,
and the invariants, stated so that each can be checked and failed.

## Terms

Where a term also names something in the orchestrator, the code meaning wins
and this document follows it.

### What ships in a challenge

| Term | Meaning |
|---|---|
| **Runtime** | the pinned upstream application: its code, its authorization logic, and the default protection it gives each asset class. Inherited, never authored. |
| **Deployment** | the Dockerfile, entrypoint, and provisioning that stand the application up and seed its state. Runs once at bring-up and is allowed to change the application. |
| **Vulbox probe** | the per-challenge code the orchestrator calls during a match, in five roles: placement, capture, integrity, functional, restart. May change transient state; may not create or protect an asset. |
| **Facility** | the role the orchestrator and its probes play against a running challenge: the privileged principal that plants canaries and observes outcomes. |

### The three layers

| Term | Meaning |
|---|---|
| **Interface** | the application's public surface, where the attacker starts. |
| **Privileged actions** | the typed operations whose protection the application authors. Where cells live and where the facility plants. |
| **Infrastructure** | container, filesystem, SSH, network, orchestrator. Neither a scoring surface nor cell material. |

### What is measured

| Term | Meaning |
|---|---|
| **Asset** | something the application protects — a credential store, another tenant's records, the service process. |
| **Boundary** | the application-established separation guarding an asset from the baseline identity. |
| **Asset graph** | the inventory of an application's assets and the boundaries guarding them, inferred during triage and confirmed at runtime. |
| **Attack scope** | a registered, application-independent name for one boundary crossing, such as reading a credential store or writing across a tenant boundary. |
| **Cell** | one asset behind one boundary in one application, carrying an attack scope. The unit a challenge declares. |
| **Capability** | the independently scored outcome of reaching a cell, reported under that cell's attack scope. |
| **Canary** | the rotating value the facility plants or issues to measure a cell. An instrument, never an asset: deleting it leaves the asset intact. |
| **Mark** | the per-attacker, per-round canary used by observed oracles, supplying attribution and replay resistance. |
| **Read oracle** | proves a cell by recovery: the facility plants a canary, the attacker submits the value. |
| **Observed oracle** | proves a cell by observation: the facility issues a mark, the attacker places it in protected state, the facility observes it privileged-side. Covers write and execution outcomes. |
| **Baseline identity** | the attacker's starting principal and everything that principal may legitimately do. |
| **Functional core** | the minimum declared workflows a defense must preserve. |
| **Floor witness** | the advisory-backed path proving at least one cell is reachable at the pinned revision. |
| **Reproduction / escalation** | per-instance labels read off the score vector: cells the floor witness reaches, and everything else. |
| **Genuine path** | a path crossing a boundary that exists in the upstream application, as against an **arena-specific path** that depends on fixtures the challenge introduced. |


## The invariants

Everything the rest of this site argues for reduces to the properties below.
The Model, Runtime, and Authoring pages give the reasoning; this section is
the contract.

### A. Application and cells

- **A1.** The application is a pinned upstream revision. Its runtime and
  authorization logic are inherited and never authored.
- **A2.** A cell is exactly one asset combined with one registered attack
  scope, in one application.
- **A3.** A cell exists only when all three hold: the application holds the
  asset, the boundary guarding it was established by the application, and
  the application exposes a typed operation the facility can plant through.
- **A4.** No cell lies within the baseline identity's legitimate reach.
- **A5.** Every asset class the application exposes is either assigned its
  cells or excluded with a recorded reason.
- **A6.** No cell's proof material is contained in or derived from another
  cell's.
- **A7.** Cells are derived from the asset graph and never from the
  vulnerability.
- **A8.** A cell declaration names the asset, its attack scope, its oracle
  kind, and its capture state. It never names the store.

### B. Baseline identity

- **B1.** Each challenge defines its baseline identity explicitly, and it is
  the same for every attacker.
- **B2.** Everything within that identity's legitimate reach is interface,
  and reaching it is not a capability.

### C. Proof material

- **C1.** Every round issues fresh proof material for every cell, and only
  the current round's material may be credited.
- **C2.** Material differs per round, per cell, and per victim. Observed
  material additionally differs per attacker.
- **C3.** Read canaries are unpredictable: drawn randomly, never derived
  from round, team, or cell identifiers.
- **C4.** An execution cell's observed region is reachable only by
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
- **D5.** Determinism covers the layout only. Canary values remain fresh
  randomness.

### E. Layers

- **E1.** Placement acts only through the application's own mechanism, at
  the privilege that mechanism requires.
- **E2.** After planting, the challenge has added no protection of its own.
  If it set the location, the mode, or the ownership, the cell is invalid.
  Protection a cell depends on that appears in the challenge's provisioning
  and nowhere upstream is authored, and separation that exists only as
  deployment topology cannot be rebuilt inside the box.
- **E3.** Capture reads from a vantage neither party controls, and reads the
  representation the application itself treats as authoritative.
- **E4.** Functional checks use only the application's user-facing
  interface.
- **E5.** Infrastructure is neither a scoring surface nor cell material.

### F. Probes and state

- **F1.** A probe may change transient state. It may not create an asset or
  add protection to one.
- **F2.** Placement is convergent: run from any prior state, it lands on the
  declared state.
- **F3.** The round proceeds in one order — observe integrity, converge
  context, rotate canaries, arm observed regions.
- **F4.** Durable state that a later round depends on belongs to deployment,
  never to a probe.
- **F5.** Derivable state is restored and never scored. Non-derivable state
  is scored, and is therefore kept to one canary per cell.

### G. Measurement and recording

- **G1.** The runtime records and does not judge. No component decides a
  verdict during a match.
- **G2.** Retry is the only runtime response to an inconclusive
  measurement.
- **G3.** Every measurement is three-valued and is recorded with its reason,
  including the inconclusive case.
- **G4.** The record preserves reached-this-round against retained from an
  earlier round, never-reached against no-longer-present, and defender
  damage against an unreadable box.
- **G5.** Availability is telemetry rather than a penalty applied at
  runtime.
- **G6.** The log is facility-side, append-only, and not derivable from
  anything inside the box.
- **G7.** No capability is creditable from evidence that either party can
  read, forge, or replay, and none from a single memorized constant.

### H. Functional core

- **H1.** The declared functional core is the whole of the defender's
  rulebook. Any change preserving it is a legitimate defense.
- **H2.** The deep checker covers the features on every materially distinct
  cell-reaching chain known at authoring time.
- **H3.** Each run is randomized and provisions its own participants, and
  asserts semantic outcomes rather than fixed strings.
- **H4.** The checker is two-sided: amputation and canned responses fail it,
  and an equivalent narrow hardening passes it.
- **H5.** The checker runs outside the defender's editable surface.

### I. Floor and interpretation

- **I1.** The floor witness proves at least one cell is reachable at the
  pinned revision. It does not define the cell set.
- **I2.** Reproduction and escalation are per-instance labels read off the
  record after the fact, not properties of a cell or a scope.
- **I3.** Whether a path is genuine or arena-specific is judged against the
  recorded asset graph.

### Where each group is enforced

| Group | Enforced at |
|---|---|
| A, B, I1, I3 | curation and authoring review |
| C, D, E, F | static verification and framework contract |
| G | orchestrator implementation |
| H | authoring review, exercised every round |
