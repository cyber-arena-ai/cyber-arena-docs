# Judging candidate promise

## Judging candidate promise

Escalation potential is a property of the primitive, not of the report. An
advisory records what someone did; the repair records what the attacker
controls. Curation should therefore ask the same three questions of every
candidate:

1. **What does the primitive control?** From the exact repair, identify
   which attacker input reaches which sink and which constraint the fix
   restored. This yields the primitive's reach set.
2. **What does the reach set intersect?** Join the reach set against the
   application's declared cells. A non-empty intersection beyond the floor cell
   is headroom.
3. **Is there one nameable composition step?** Does some reachable cell
   hold material — a credential, configuration value, administrative
   object, internal endpoint, or loadable path — that unlocks a further
   cell?

Reach sets follow from the primitive class:

| Primitive identified in the repair | Reach set | Typical composition step |
|---|---|---|
| Missing or incorrect authorization check | Whatever that authority guards | Follow the role graph toward privileged actions |
| Object reference control | The whole object class | The class contains secrets or administrative objects |
| Path control | Host file read or write | Write into a served or auto-loaded path |
| URL or host control | Internal network services | The internal service holds its own credentials |
| Query or expression control | Application data read and write | Credential store, or engine file and exec sinks |
| Deserialization or command construction | The service process | Already at the ceiling |
| Token or cryptographic weakness | Identity assumption | Whatever that identity holds |
| Upload or write primitive | Host file write | Written into a code, template, or configuration path |

A candidate is promising when the second question has a non-empty answer
and the third is answerable. Naming one composition step is sufficient;
proving it belongs to runtime study. Severity, CVSS, exploitation records,
and exploit-module availability are not answers to any of the three
questions.

These questions are answerable from the repair, the application's data and
permission model, and a bounded sink inventory. They deliberately do not
require a build, an execution, or a full source trace.

## Floor at the ceiling

A primitive that lands directly on the service process has no headroom,
because every lower cell is then trivially reachable and the ladder
collapses to one meaningful step. This must not disqualify execution
challenges. When the floor cell is the ceiling, the headroom requirement is
waived and the entire difficulty burden shifts to the defense: blocking the
path must require restoring an invariant while preserving the functional
core.

Gitea 1.16.6 fails exactly there. Its floor is at the ceiling, so there is
no escalation to measure, and its repair is an option terminator on one
`git fetch` call, so there is no defensive difficulty either. A shallow
attack chain remains acceptable when the cells above it are deep; when
nothing is above it, the defense must carry the challenge alone.

## Potential candidate-source enhancement

OSV should remain the broad vulnerability and Git-range backbone. Candidate
shortlisting could additionally join five continuously maintained evidence
sources:

- **CISA Known Exploited Vulnerabilities:** evidence of exploitation in the
  wild.
- **CISA Vulnrichment:** structured exploitation, automation, and technical
  impact assessments across a broader CVE pool.
- **Reviewed Nuclei templates:** executable network behavior, prerequisites,
  and concrete exploit matchers.
- **Metasploit modules:** corroboration that a reliable exploit and tested
  target configuration exist.
- **Repository or vendor advisories:** maintainer context about the affected
  workflow and repair.

These sources should provide independent prioritization signals, not replace
OSV or become admission gates. Their presence does not establish challenge
depth; it identifies candidates that deserve closer semantic and runtime
study.

The three questions must be answered to the same standard whether or not
these records exist. They may shorten the work — a tested module names the
sink, and evidence of exploitation in the wild corroborates that
prerequisites arise in a default configuration — but they may not stand in
for the reach set, the intersected cells, or the composition step, and they
may not lift a verdict when the intersection is empty. Their absence is not
evidence against a candidate: a repair showing query control in an
application with a credential store is promising on identical footing to
one carrying all five corroborating records.

Curation artifacts should therefore record the same fields regardless of
source, leaving only the cited evidence to differ. When a conclusion can be
asserted only by citing a corroborating record rather than by filling those
fields, the candidate is unresolved at this depth and belongs in runtime
study.
