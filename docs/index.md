# Cyber Arena challenge design

Cyber Arena should measure how far an agent can advance through a general
security-capability ladder when operating against a realistic application,
without prescribing the vulnerability or attack path it must use.

A challenge is therefore an application and its security boundaries, not an
exercise for one intended CVE. A known vulnerability establishes that the
selected historical application is a viable target, but it does not define the
only valid solution or the highest capability an agent may discover. Genuine
application vulnerabilities remain valid even when they were not anticipated
by the challenge author.

Success should represent concrete security effects, such as crossing an
application boundary, acquiring authority, reaching protected resources, or
controlling service execution. These effects should be comparable across
applications while allowing each application to expose a different reachable
subset. The benchmark should preserve the observed capabilities rather than
infer them from the vulnerability used.

The application must remain meaningfully functional during evaluation.
Defensive changes should be allowed, but replacing the service with a canned
response or disabling required behavior must not count as a successful
defense. Functionality checks should preserve the application's declared
service behavior without encoding a particular exploit path.

Scored capabilities must be reached through the application boundary, not
through challenge infrastructure, leaked verifier state, or direct access to
planted evidence. Capability evidence and functionality checks should be
independent of the mutable application so that they measure the application
rather than artifacts of the benchmark harness.

The resulting benchmark has a confirmed viable floor but an open ceiling:
curation demonstrates that a meaningful security boundary can be crossed,
while evaluation remains free to reveal stronger capabilities as models
improve.


## Reading order

Read [the challenge model](challenge-model.md) first. It is the only
normative page: it defines what a challenge is, section by section, and
closes each section with the invariants that make it checkable. Everything
else on this site and in the orchestrator elaborates it.

Several of its terms — objective, proof token, facility, noise — are narrower here
than in ordinary use. The challenge model defines them where they are
introduced, and the site shows each definition on hover wherever the term
appears.

**Authoring guidance is forthcoming.**
