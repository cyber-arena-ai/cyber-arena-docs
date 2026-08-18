# Glossary

The terms in [the challenge model](challenge-model.md) carry exactly these
meanings. The model governs: where the orchestrator uses a term differently,
that is a defect in the code, not a second definition.

## Match and roles

| Term | Meaning |
|---|---|
| **Match** | one complete run of a challenge between all participating teams, divided into rounds. |
| **Round** | the unit the facility rotates on: one pass of observe integrity, converge, rotate canaries, arm regions, with a capture window against live opponents. |
| **Phase** | a declared window within a round during which particular actions are accepted; phase control is orchestrator-owned. |
| **Team** | one participant, simultaneously **attacker** against every other team's box and **victim** of theirs. |
| **Agent** | the model-driven process acting for a team, whose action surface is exactly the declared tool set. |
| **Match secret** | the facility-held key that seeds per-team derivation, so no team can derive another's layout. |
| **Claim** | an attacker's assertion that an objective was reached, submitted in a declared phase and answered from a closed result vocabulary. |
| **Availability** | whether a box served its declared interface, sampled by the poller and recorded as telemetry rather than priced during the match. |

## Challenge composition

| Term | Meaning |
|---|---|
| **Runtime** | the pinned upstream application — its code, its authorization logic, and the default protection it gives each asset class — inherited, never authored. |
| **Deployment** | the Dockerfile, entrypoint, and provisioning that stand the application up, running once at bring-up and allowed to change the application. |
| **Vulbox probe** | the per-challenge code the orchestrator calls during a match, in five roles — placement, capture, integrity, SLA, restart — which may change transient state but may not create or protect an asset. |
| **Facility** | the role the orchestrator and its probes play against a running challenge: the privileged principal that plants canaries and observes outcomes. |
| **Flag handler** | the per-challenge code that creates seed data and plants and rotates canaries in it, through application operations only. |
| **Liveness check** | the shallow probe that establishes a service is reachable. |
| **SLA checker** | the check that exercises the declared SLA and decides whether a defense is legitimate. |
| **Arm** | to prepare an observed region for the round, so that a mark placed in it can be observed. |

## Access layers

| Term | Meaning |
|---|---|
| **Public surface** | everything the application offers the baseline identity, where the attacker starts. |
| **Guarded surface** | everything the application gates behind authority it enforces itself, where objectives live and where the facility plants. |
| **Infrastructure** | container, filesystem, SSH, network, orchestrator — neither a scoring surface nor objective material. |

## Targets

| Term | Meaning |
|---|---|
| **Application operation** | an action the application exposes at the public or guarded surface, never at infrastructure, where the application's own code decides where the data lands and what protects it. |
| **Asset** | something of value the application holds and restricts access to, such as a credential store, another tenant's records, or the service process. |
| **Trust boundary** | the application-established separation guarding an asset from the baseline identity; the recorded set of them is the challenge's inventory of what is protected and what is public, inferred during triage and confirmed at runtime. |
| **Attack scope** | a registered, application-independent name for one boundary crossing, such as reading a credential store or writing across a tenant boundary. |
| **Objective** | exactly one asset under exactly one attack scope, in one application; the unit a challenge declares. |
| **Baseline identity** | the attacker's starting principal and everything that principal may legitimately do. |

## Evidence

| Term | Meaning |
|---|---|
| **Canary** | the fresh value that proves an objective in one round — planted for a read objective and recovered by the attacker, or issued to the attacker for a write or execute objective and placed by them. |
| **Mark** | a canary issued to one attacker rather than planted, so that placing it attributes the write or execution to that attacker. |
| **Capture** | the event that proves an objective was reached: a canary submitted, or a mark observed in the protected region. |
| **Capability** | the independently scored outcome of reaching an objective, reported under that objective's attack scope. |
| **Read oracle** | proves an objective by recovery: the facility plants a canary, the attacker submits the value. |
| **Observed oracle** | proves an objective by observation — the facility issues a mark, the attacker places it in protected state, and the facility observes it privileged-side — covering write and execution outcomes. |
| **Canary ID** | the pointer an objective publishes: where the canary is planted for a read objective, or where it must be placed for a write or execute objective. Called `flag_id` in the orchestrator and `public_flag_id` in other attack/defense frameworks. |
| **Observed region** | the state the facility watches for a write or execute objective, and where the attacker must place the mark. |

## Environment and defense

| Term | Meaning |
|---|---|
| **Seed data** | the application state the flag handler creates for a challenge — identities, records, entries — inside which canaries are planted. |
| **Noise** | data the checker creates and exercises for itself through the public surface — never scored, never seed data, and re-created from any prior state. |
| **SLA** | the minimum declared workflows a defense must keep serving; the standard attack/defense term for the service-availability contract. |

## Interpretation

| Term | Meaning |
|---|---|
| **Floor witness** | the advisory-backed path proving at least one objective is reachable at the pinned revision. |
| **Reproduction / escalation** | per-instance labels read off the score vector: objectives the floor witness reaches, and everything else. |
| **Genuine path** | a path crossing a boundary that exists in the upstream application, as against an **arena-specific path** that depends on fixtures the challenge introduced. |
