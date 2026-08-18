*[cell]: One asset combined with one registered attack scope, in one application. The unit a challenge declares.
*[Cell]: One asset combined with one registered attack scope, in one application. The unit a challenge declares.
*[cells]: One asset combined with one registered attack scope, in one application. The unit a challenge declares.
*[Cells]: One asset combined with one registered attack scope, in one application. The unit a challenge declares.

*[canary]: The rotating value the facility plants or issues to measure a cell. An instrument, never an asset.
*[Canary]: The rotating value the facility plants or issues to measure a cell. An instrument, never an asset.
*[canaries]: The rotating value the facility plants or issues to measure a cell. An instrument, never an asset.

*[mark]: The per-attacker, per-round canary used by observed oracles, supplying attribution and replay resistance.
*[marks]: The per-attacker, per-round canary used by observed oracles, supplying attribution and replay resistance.

*[facility]: The role the orchestrator and its probes play against a running challenge: the privileged principal that plants canaries and observes outcomes.
*[Facility]: The role the orchestrator and its probes play against a running challenge: the privileged principal that plants canaries and observes outcomes.

*[attack scope]: A registered, application-independent name for one boundary crossing. The unit that makes results comparable between challenges.
*[Attack scope]: A registered, application-independent name for one boundary crossing. The unit that makes results comparable between challenges.
*[attack scopes]: A registered, application-independent name for one boundary crossing. The unit that makes results comparable between challenges.

*[baseline identity]: The attacker's starting principal and everything that principal may legitimately do.
*[Baseline identity]: The attacker's starting principal and everything that principal may legitimately do.

*[asset graph]: The inventory of an application's assets and the boundaries guarding them, inferred during triage and confirmed at runtime.
*[Asset graph]: The inventory of an application's assets and the boundaries guarding them, inferred during triage and confirmed at runtime.

*[floor witness]: The advisory-backed path proving at least one cell is reachable at the pinned revision.
*[Floor witness]: The advisory-backed path proving at least one cell is reachable at the pinned revision.

*[functional core]: The minimum declared workflows a defense must preserve.
*[Functional core]: The minimum declared workflows a defense must preserve.

*[read oracle]: Proves a cell by recovery: the facility plants a canary, the attacker submits the value.
*[Read oracle]: Proves a cell by recovery: the facility plants a canary, the attacker submits the value.

*[observed oracle]: Proves a cell by observation: the facility issues a mark, the attacker places it in protected state, the facility observes it privileged-side.
*[Observed oracle]: Proves a cell by observation: the facility issues a mark, the attacker places it in protected state, the facility observes it privileged-side.
*[observed oracles]: Proves a cell by observation: the facility issues a mark, the attacker places it in protected state, the facility observes it privileged-side.

*[vulbox probe]: The per-challenge code the orchestrator calls during a match, in five roles: placement, capture, integrity, functional, restart.
*[Vulbox probe]: The per-challenge code the orchestrator calls during a match, in five roles: placement, capture, integrity, functional, restart.

*[asset]: Something of value the application holds and restricts access to, such as a credential store, another tenant's records, or the service process.
*[Asset]: Something of value the application holds and restricts access to, such as a credential store, another tenant's records, or the service process.
*[assets]: Something of value the application holds and restricts access to, such as a credential store, another tenant's records, or the service process.

*[boundary]: The application-established separation guarding an asset from the baseline identity.
*[Boundary]: The application-established separation guarding an asset from the baseline identity.
*[boundaries]: The application-established separation guarding an asset from the baseline identity.

*[capability]: The independently scored outcome of reaching a cell, reported under that cell's attack scope.
*[Capability]: The independently scored outcome of reaching a cell, reported under that cell's attack scope.
*[capabilities]: The independently scored outcome of reaching a cell, reported under that cell's attack scope.

*[interface]: The application's public surface, where the attacker starts.
*[Interface]: The application's public surface, where the attacker starts.

*[privileged actions]: The typed operations whose protection the application authors, where cells live and where the facility plants.
*[Privileged actions]: The typed operations whose protection the application authors, where cells live and where the facility plants.

*[infrastructure]: Container, filesystem, SSH, network, orchestrator — neither a scoring surface nor cell material.
*[Infrastructure]: Container, filesystem, SSH, network, orchestrator — neither a scoring surface nor cell material.

*[runtime]: The pinned upstream application: its code, its authorization logic, and the default protection it gives each asset class.
*[Runtime]: The pinned upstream application: its code, its authorization logic, and the default protection it gives each asset class.

*[deployment]: The Dockerfile, entrypoint, and provisioning that stand the application up and seed its state.
*[Deployment]: The Dockerfile, entrypoint, and provisioning that stand the application up and seed its state.

*[genuine path]: A path crossing a boundary that exists in the upstream application, as against an arena-specific path that depends on fixtures the challenge introduced.
*[arena-specific path]: A path that depends on fixtures, secrets, interfaces, or behavior introduced by the challenge environment.

*[reproduction]: The per-instance label for cells the floor witness reaches, read off the record after the fact.
*[escalation]: The per-instance label for cells beyond those the floor witness reaches, read off the record after the fact.
