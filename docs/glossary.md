# Glossary

The terms in [the challenge model](challenge-model.md) carry exactly these
meanings. Where a term also names something in the orchestrator, the code
meaning wins and this page follows it.

## Challenge composition

| Term | Meaning |
|---|---|
| **Runtime** | the pinned upstream application — its code, its authorization logic, and the default protection it gives each asset class — inherited, never authored. |
| **Deployment** | the Dockerfile, entrypoint, and provisioning that stand the application up, running once at bring-up and allowed to change the application. |
| **Vulbox probe** | the per-challenge code the orchestrator calls during a match, in five roles — placement, capture, integrity, functional, restart — which may change transient state but may not create or protect an asset. |
| **Facility** | the role the orchestrator and its probes play against a running challenge: the privileged principal that plants canaries and observes outcomes. |

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
| **Boundary** | the application-established separation guarding an asset from the baseline identity. |
| **Asset graph** | the inventory of an application's assets and the boundaries guarding them, inferred during triage and confirmed at runtime. |
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
| **Locator** | the pointer an objective publishes: where the canary is planted for a read objective, or where it must be placed for a write or execute objective. |
| **Observed region** | the state the facility watches for a write or execute objective, and where the attacker must place the mark. |

## Environment and defense

| Term | Meaning |
|---|---|
| **Seed data** | the application state the flag handler creates for a challenge — identities, records, entries — inside which canaries are planted. |
| **Noise** | data the checker creates and exercises for itself through the public surface — never scored, never seed data, and re-created from any prior state. |
| **Functional core** | the minimum declared workflows a defense must preserve. |

## Interpretation

| Term | Meaning |
|---|---|
| **Floor witness** | the advisory-backed path proving at least one objective is reachable at the pinned revision. |
| **Reproduction / escalation** | per-instance labels read off the score vector: objectives the floor witness reaches, and everything else. |
| **Genuine path** | a path crossing a boundary that exists in the upstream application, as against an **arena-specific path** that depends on fixtures the challenge introduced. |
