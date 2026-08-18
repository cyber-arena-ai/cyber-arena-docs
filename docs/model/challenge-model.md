# The challenge model

A challenge is a pinned application revision together with five things the
benchmark measures against it:

- **Actor model:** the attacker's baseline identity and everything that
  identity may legitimately do.
- **Asset graph:** the protected targets the application holds, each behind
  a named enforced boundary.
- **Objectives:** independently scored outcomes defined over the asset
  graph, each carrying a registered attack scope.
- **Functional core:** the minimum declared workflows a defense must
  preserve.
- **Floor witness:** the advisory-backed path proving at least one objective is
  reachable at that revision.

The capability objectives are defined against the asset graph and never against
the vulnerability. The floor witness proves the objective set is non-empty; it
does not define it. This is what allows a genuine unanticipated path to
score exactly like the selected one.

## The baseline identity

Every objective's existence test is stated relative to the baseline identity, so
that identity has to be defined before any objective can be. It is the trust
boundary of a normal user of that application: the principal a legitimate
deployment would hand to someone with no special standing.

It is defined per challenge rather than by the arena, because what a normal
user is differs by application — an anonymous visitor for a public service,
a registered account for a collaboration tool, a member of one project for a
multi-tenant system. The arena fixes only that the definition exists, is
written down, and is the same for every attacker
([B1](../invariants.md#b-trust-boundary)).

Two things follow. Everything the baseline identity may legitimately do is
interface, so no objective may sit inside its reach. And every capability is
measured as a distance from it, which is what lets applications with
different notions of a normal user still produce comparable results: the
scope names the crossing, and the baseline names where the crossing starts.
