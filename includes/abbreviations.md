*[objective]: One asset combined with one registered attack scope, in one application. The unit a challenge declares.
*[Objective]: One asset combined with one registered attack scope, in one application. The unit a challenge declares.
*[objectives]: One asset combined with one registered attack scope, in one application. The unit a challenge declares.
*[Objectives]: One asset combined with one registered attack scope, in one application. The unit a challenge declares.

*[canary]: The rotating value the facility plants or issues to measure an objective. An instrument, never an asset.
*[Canary]: The rotating value the facility plants or issues to measure an objective. An instrument, never an asset.
*[canaries]: The rotating value the facility plants or issues to measure an objective. An instrument, never an asset.

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

*[floor witness]: The advisory-backed path proving at least one objective is reachable at the pinned revision.
*[Floor witness]: The advisory-backed path proving at least one objective is reachable at the pinned revision.

*[functional core]: The minimum declared workflows a defense must preserve.
*[Functional core]: The minimum declared workflows a defense must preserve.

*[read oracle]: Proves an objective by recovery: the facility plants a canary, the attacker submits the value.
*[Read oracle]: Proves an objective by recovery: the facility plants a canary, the attacker submits the value.

*[observed oracle]: Proves an objective by observation: the facility issues a mark, the attacker places it in protected state, the facility observes it privileged-side.
*[Observed oracle]: Proves an objective by observation: the facility issues a mark, the attacker places it in protected state, the facility observes it privileged-side.
*[observed oracles]: Proves an objective by observation: the facility issues a mark, the attacker places it in protected state, the facility observes it privileged-side.

*[vulbox probe]: The per-challenge code the orchestrator calls during a match, in five roles: placement, capture, integrity, functional, restart.
*[Vulbox probe]: The per-challenge code the orchestrator calls during a match, in five roles: placement, capture, integrity, functional, restart.

*[asset]: Something of value the application holds and restricts access to, such as a credential store, another tenant's records, or the service process.
*[Asset]: Something of value the application holds and restricts access to, such as a credential store, another tenant's records, or the service process.
*[assets]: Something of value the application holds and restricts access to, such as a credential store, another tenant's records, or the service process.

*[boundary]: The application-established separation guarding an asset from the baseline identity.
*[Boundary]: The application-established separation guarding an asset from the baseline identity.
*[boundaries]: The application-established separation guarding an asset from the baseline identity.

*[capability]: The independently scored outcome of reaching an objective, reported under that objective's attack scope.
*[Capability]: The independently scored outcome of reaching an objective, reported under that objective's attack scope.
*[capabilities]: The independently scored outcome of reaching an objective, reported under that objective's attack scope.



*[infrastructure]: Container, filesystem, SSH, network, orchestrator — neither a scoring surface nor objective material.
*[Infrastructure]: Container, filesystem, SSH, network, orchestrator — neither a scoring surface nor objective material.



*[genuine path]: A path crossing a boundary that exists in the upstream application, as against an arena-specific path that depends on fixtures the challenge introduced.
*[arena-specific path]: A path that depends on fixtures, secrets, interfaces, or behavior introduced by the challenge environment.

*[reproduction]: The per-instance label for objectives the floor witness reaches, read off the record after the fact.
*[escalation]: The per-instance label for objectives beyond those the floor witness reaches, read off the record after the fact.

<!-- Deliberately NOT auto-linked: runtime and deployment.
     Both are defined terms, but most of their occurrences in this site use the
     ordinary sense ("confirmed at runtime", "a legitimate deployment"), and the
     abbr extension marks EVERY occurrence with no way to exclude one. A tooltip
     that is wrong more often than right is worse than none. Both remain defined
     in the terminology table. -->

*[capture]: The event that proves an objective was reached: a canary submitted, or a mark observed in the protected region.
*[Capture]: The event that proves an objective was reached: a canary submitted, or a mark observed in the protected region.
*[captures]: The event that proves an objective was reached: a canary submitted, or a mark observed in the protected region.

*[seed data]: The application state the flag handler creates for a challenge — identities, records, entries — inside which canaries are planted.
*[Seed data]: The application state the flag handler creates for a challenge — identities, records, entries — inside which canaries are planted.

*[application operation]: An action the application exposes at the interface or privileged-actions layer, never at infrastructure, where the application's own code decides where the data lands and what protects it.
*[application operations]: An action the application exposes at the interface or privileged-actions layer, never at infrastructure, where the application's own code decides where the data lands and what protects it.
*[Application operation]: An action the application exposes at the interface or privileged-actions layer, never at infrastructure, where the application's own code decides where the data lands and what protects it.

*[public surface]: Everything the application offers the baseline identity, where the attacker starts.
*[Public surface]: Everything the application offers the baseline identity, where the attacker starts.
*[guarded surface]: Everything the application gates behind authority it enforces itself, where objectives live and where the facility plants.
*[Guarded surface]: Everything the application gates behind authority it enforces itself, where objectives live and where the facility plants.

*[noise]: Data the checker creates and exercises for itself through the public surface — never scored, never seed data.
*[Noise]: Data the checker creates and exercises for itself through the public surface — never scored, never seed data.

*[locator]: The pointer an objective publishes: where the canary is planted for a read objective, or where it must be placed for a write or execute objective.
*[Locator]: The pointer an objective publishes: where the canary is planted for a read objective, or where it must be placed for a write or execute objective.
*[observed region]: The state the facility watches for a write or execute objective, and where the attacker must place the mark.
*[Observed region]: The state the facility watches for a write or execute objective, and where the attacker must place the mark.

*[flag handler]: The per-challenge code that creates seed data and plants and rotates canaries in it, through application operations only.
*[Flag handler]: The per-challenge code that creates seed data and plants and rotates canaries in it, through application operations only.
*[deep checker]: The check that exercises the declared functional core and decides whether a defense is legitimate.
*[Deep checker]: The check that exercises the declared functional core and decides whether a defense is legitimate.
*[match secret]: The facility-held key that seeds per-team derivation, so no team can derive another's layout.
