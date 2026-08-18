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
- **A2.** An objective exists only in assets the application owns, never in
  infrastructure.
- **A3.** An objective proves a security capability was reached against the
  application, never the attack chain that reached it.
- **A4.** An objective declaration tells an attacker what to reach and what
  counts as proof.
- **A5.** An objective may be a read, write, or execute operation.
- **A6.** The application is stateful, and both parties can damage that
  state.

### B. Trust boundary

- **B1.** Every attacker starts from the same declared baseline identity.
- **B2.** Every objective lies outside the baseline identity's boundary.

### C. Proof and credit

- **C1.** Every round issues a fresh canary for each objective.
- **C2.** Only the current round's canary may be credited.

- **C3.** A canary is unguessable and unforgeable.

- **C4.** An issued canary identifies the attacker it was issued to.
- **C5.** An execute objective's observed region is reachable only by
  execution, not by writing.

- **C6.** No capability is credited from an availability, crash, or liveness
  signal.

### D. Seed data

- **D1.** The seed data is a pure function of a per-team seed.
- **D2.** No team can derive another team's seed data.
- **D3.** The flag handler creates the seed data, and its layout is
  identical in every round.


### E. Facility conduct

- **E1.** The flag handler seeds and plants only through the application's own
  operations.

- **E2.** An objective's protection is established by the application alone.
- **E3.** Capture reads the application's authoritative state from a vantage
  neither party controls.
- **E4.** Functional checks act only through the application's user-facing
  interface.

- **E5.** Placement is convergent: from any prior state it lands on the
  declared state.
- **E6.** The round proceeds in one order — observe integrity, converge
  context, rotate canaries, arm observed regions.
- **E7.** Derivable state is restored and never scored, while non-derivable
  state is scored and kept to one canary per objective.
- **E8.** Restart is idempotent: repeated restarts converge on the same
  serving state.

### F. Recording

- **F1.** The orchestrator records and never judges during a match.
- **F2.** Retry is the only in-match response to an inconclusive measurement.
- **F3.** Every measurement is recorded with its outcome and its reason,
  including when nothing could be determined.
- **F4.** The log is append-only.

### G. Functional core

- **G1.** The declared functional core is the whole of the defender's
  rulebook, so any change preserving it is a legitimate defense.
- **G2.** The deep checker covers the features on every materially distinct
  objective-reaching chain known at authoring time.
- **G3.** Each run is randomized, provisions its own participants, and
  asserts semantic outcomes rather than fixed strings.
- **G4.** The checker is two-sided: amputation and canned responses fail it,
  and an equivalent narrow hardening passes it.
- **G5.** The checker runs outside the defender's editable surface.

### H. Interpretation

- **H1.** The floor witness proves at least one objective is reachable at the
  pinned revision but does not define the objective set.
- **H2.** Reproduction and escalation are labels read off the record after
  the fact, not properties of an objective or a scope.
- **H3.** Whether a path is genuine or arena-specific is judged against the
  recorded asset graph.

### I. Infrastructure

- **I1.** A team reaches another team's box only through that challenge's
  declared public interface.
- **I2.** The facility's privileged channel belongs to neither party's
  surface, and a box it cannot reach is recorded as unavailable.
- **I3.** Facility state — canaries, marks, seeds, and the log — lives
  outside the boxes and is derivable from none of them.
- **I4.** Phase control is orchestrator-owned, so claims and submissions are
  accepted only in declared phases.
- **I5.** An agent's action surface is exactly the declared tool set, with no
  unmediated path to another team's box or to the orchestrator.
- **I6.** Every team receives the same image, the same resource limits, and
  the same schedule.
- **I7.** The defender's mutation path is orchestrator-mediated, so every
  edit and restart is observable in the log.
- **I8.** An orchestrator fault is recorded as an orchestrator fault, never
  as a team's outcome.
- **I9.** One container per service, and a challenge exposes one public
  interface.

### Where each group is enforced

| Group | Enforced at |
|---|---|
| A, B, H | curation and authoring review |
| C, D, E | static verification and framework contract |
| F, I | orchestrator implementation |
| G | authoring review, exercised every round |

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
| **Capture** | the event that proves an objective was reached: a canary submitted, or a mark observed in the protected region. |
| **Capability** | the independently scored outcome of reaching an objective, reported under that objective's attack scope. |
| **Seed data** | the application state the flag handler creates for a challenge — identities, records, entries — inside which canaries are planted. |
| **Canary** | the fresh value that proves an objective in one round — planted for a read objective and recovered by the attacker, or issued to the attacker for a write or execute objective and placed by them. |
| **Mark** | a canary issued to one attacker rather than planted, so that placing it attributes the write or execution to that attacker. |
| **Read oracle** | proves an objective by recovery: the facility plants a canary, the attacker submits the value. |
| **Observed oracle** | proves an objective by observation — the facility issues a mark, the attacker places it in protected state, and the facility observes it privileged-side — covering write and execution outcomes. |
| **Baseline identity** | the attacker's starting principal and everything that principal may legitimately do. |
| **Functional core** | the minimum declared workflows a defense must preserve. |
| **Floor witness** | the advisory-backed path proving at least one objective is reachable at the pinned revision. |
| **Reproduction / escalation** | per-instance labels read off the score vector: objectives the floor witness reaches, and everything else. |
| **Genuine path** | a path crossing a boundary that exists in the upstream application, as against an **arena-specific path** that depends on fixtures the challenge introduced. |

