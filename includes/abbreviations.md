*[objective]: One protected asset under one attack scope.
*[Objective]: One protected asset under one attack scope.
*[objectives]: Protected assets under attack scopes.
*[Objectives]: Protected assets under attack scopes.

*[objective instance]: The concrete protected identity, record, resource, or operation realizing an objective for one victim and round.
*[Objective instance]: The concrete protected identity, record, resource, or operation realizing an objective for one victim and round.
*[objective instances]: Concrete protected identities, records, resources, or operations realizing objectives for particular victims and rounds.

*[proof token]: A facility-generated value used by an oracle as evidence that an objective was reached.
*[Proof token]: A facility-generated value used by an oracle as evidence that an objective was reached.
*[proof tokens]: Facility-generated values used by oracles as evidence that objectives were reached.

*[facility]: The trusted role that creates round context, places proof material, observes outcomes, and records measurements.
*[Facility]: The trusted role that creates round context, places proof material, observes outcomes, and records measurements.

*[attack scope]: An application-independent class of security outcome that crosses an application-enforced boundary and maps to exactly one MITRE CWE Technical Impact.
*[Attack scope]: An application-independent class of security outcome that crosses an application-enforced boundary and maps to exactly one MITRE CWE Technical Impact.
*[attack scopes]: Application-independent classes of security outcomes that cross application-enforced boundaries and map to MITRE CWE Technical Impacts.

*[baseline principal]: An application identity with the challenge's declared normal-user privileges.
*[Baseline principal]: An application identity with the challenge's declared normal-user privileges.
*[baseline principals]: Application identities with the challenge's declared normal-user privileges.

*[baseline privileges]: Everything a baseline principal may legitimately do without crossing an application-enforced boundary.
*[Baseline privileges]: Everything a baseline principal may legitimately do without crossing an application-enforced boundary.

*[trust boundary]: The application-established separation denying the baseline principal access to an asset under its baseline privileges.
*[Trust boundary]: The application-established separation denying the baseline principal access to an asset under its baseline privileges.
*[trust boundaries]: Application-established separations denying baseline principals access to protected assets under their baseline privileges.
*[Trust boundaries]: Application-established separations denying baseline principals access to protected assets under their baseline privileges.

*[PoV]: Proof of vulnerability: an advisory-backed, in-band attack path for the pinned revision.

*[SLA]: The minimum application workflows encoded and exercised by the functional health checker that every defense must keep serving.

*[read oracle]: Proof by recovery: the facility plants a proof token in protected state and the attacker submits it.
*[Read oracle]: Proof by recovery: the facility plants a proof token in protected state and the attacker submits it.

*[observed oracle]: Proof by placement: the facility issues a proof token and observes it in protected state.
*[Observed oracle]: Proof by placement: the facility issues a proof token and observes it in protected state.
*[observed oracles]: Proof mechanisms in which the facility issues proof tokens and observes them in protected state.

*[vulbox probe]: Current framework code implementing placement, capture, integrity, health checking, or restart.
*[Vulbox probe]: Current framework code implementing placement, capture, integrity, health checking, or restart.

*[asset]: Something of value the application holds and restricts.
*[Asset]: Something of value the application holds and restricts.
*[assets]: Things of value the application holds and restricts.

*[capability]: The measured outcome of reaching an objective, reported under its attack scope.
*[Capability]: The measured outcome of reaching an objective, reported under its attack scope.
*[capabilities]: Measured outcomes of reaching objectives.

*[infrastructure]: Arena-controlled execution, networking, measurement, and control resources outside the application.
*[Infrastructure]: Arena-controlled execution, networking, measurement, and control resources outside the application.

*[genuine path]: A path crossing a trust boundary inherited from the upstream application.
*[arena-specific path]: A path whose decisive boundary or shortcut exists only because of arena infrastructure or arena-introduced protection.

*[reproduction]: A reached objective also reached by the PoV for that challenge instance.
*[escalation]: A reached objective outside the PoV's reach.

<!-- Deliberately NOT auto-linked: runtime and deployment.
     Most occurrences use their ordinary meanings, so global tooltips would be
     misleading. Both remain defined in the challenge model. -->

*[capture]: A trusted observation that an attacker satisfied one current objective oracle.
*[Capture]: A trusted observation that an attacker satisfied one current objective oracle.
*[captures]: Trusted observations that attackers satisfied current objective oracles.

*[seed data]: Application state created through application operations to instantiate the current round context.
*[Seed data]: Application state created through application operations to instantiate the current round context.

*[round context]: The principals, credentials, records, jobs, paths, and other concrete state realizing objective instances in one round.
*[Round context]: The principals, credentials, records, jobs, paths, and other concrete state realizing objective instances in one round.

*[rotation]: Replacement of round context, objective instances, target IDs, and proof tokens before a new round becomes attackable.
*[Rotation]: Replacement of round context, objective instances, target IDs, and proof tokens before a new round becomes attackable.

*[application operation]: An action exposed by the application whose own code determines where data lands and what protects it.
*[application operations]: Actions exposed by the application whose own code determines where data lands and what protects it.
*[Application operation]: An action exposed by the application whose own code determines where data lands and what protects it.

*[public surface]: Interfaces, data, and operations available to any baseline principal under those privileges.
*[Public surface]: Interfaces, data, and operations available to any baseline principal under those privileges.
*[guarded surface]: Assets and operations the application denies to a principal exercising only baseline privileges and exposes only with additional authority.
*[Guarded surface]: Assets and operations the application denies to a principal exercising only baseline privileges and exposes only with additional authority.

*[noise]: Non-scoring application data created by the checker to exercise the SLA.
*[Noise]: Non-scoring application data created by the checker to exercise the SLA.

*[target ID]: The attacker-visible locator for an objective instance: what to read, where to write, or where execution must leave evidence.
*[Target ID]: The attacker-visible locator for an objective instance: what to read, where to write, or where execution must leave evidence.
*[target IDs]: Attacker-visible locators for objective instances.
*[observed region]: The application state watched for a write or execute objective.
*[Observed region]: The application state watched for a write or execute objective.

*[flag handler]: Current framework code implementing placement and capture for challenge objectives.
*[Flag handler]: Current framework code implementing placement and capture for challenge objectives.
*[functional health checker]: The challenge component that owns and externally exercises the SLA to determine whether a defense remains legitimate.
*[Functional health checker]: The challenge component that owns and externally exercises the SLA to determine whether a defense remains legitimate.
*[match secret]: Facility-held keying material used to derive independent round context.
*[live state]: The match state in which teams attack opponents and defend their own applications concurrently.
