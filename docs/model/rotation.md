# Rotation and seeded context


Rotation is not a property of one oracle. Every path requires the same
thing, and the paths differ only in who supplies it
([C1](../invariants.md#c-proof-and-credit)):

> Every round issues a fresh canary for every objective, and only the
> current round's material may be credited.

A read objective satisfies this by planting a new value the attacker must find
again. An observed objective satisfies it by issuing a new mark to each attacker.
The read path therefore carries the work — its placement probe must re-plant
each round — while the observed path gets the property for free from
issuance, and the challenge never touches the material.

Proof material varies along four axes, and the first three are required of
every path.

| Axis | Required | Purpose |
|---|---|---|
| Round | yes | last round's proof is worthless this round |
| Objective | yes | no objective's material is derived from another's, so closing one path does not leak the rest |
| Victim | yes | material recovered from one box does not score against another |
| Attacker | observed oracles only | a mark identifies who placed it; a planted value cannot, since one value serves every attacker of that victim |

The missing attacker axis is the read oracle's known weakness. Its canary is
per victim, so two attacking teams that both recover it are
indistinguishable and one may hand it to the other. Rotation bounds that
window to a single round rather than closing it, and the asymmetry is
accepted rather than concealed: observed objectives attribute, read objectives do not.

## Seeded context defeats rehearsal, not replay

Every team runs the same application image and holds privileged access to
its own box. If the seeded data layer were identical across teams, an
attacker would read its own copy to learn exactly which record, which
principal, and which entry hold the opponent's canary, and could rehearse
the whole path offline against a perfect replica.

Varying that context is therefore a **victim-axis** property rather than a
round-axis one, and it addresses rehearsal rather than replay. It also
dissolves the apparent problem of singleton assets: an application has one
credential store, but the challenge populates it, so which entry under which
identity holds the canary still differs per team. The asset is fixed by the
application; its contents are ours.

The flag handler owns the seeded population, because the population and the
canary inside it are one concern: the handler that plants a canary into a
credential entry is the code that must know the entry exists. It creates the
population on first placement and reconciles it every round thereafter, so a
wipe is repaired rather than fatal.

The layout it creates is identical in every round. The functional core needs
a stable population to exercise, and reseeding would achieve nothing anyway:
the victim axis already denies rehearsal and the round axis already denies
replay.

## Seeding is deterministic

The seeded population must be a pure function of a per-team seed rather than
freshly sampled ([D1](../invariants.md#d-seeded-context)).

- **Derive rather than sample.** Identities, names, identifiers, and paths
  all follow from the seed by a documented function.
- **Key the derivation with a match secret.** A seed computed from the team
  identifier alone would let any team derive its opponent's layout.
- **The seed never lands on the box.** The population does; the seed that
  generated it stays with the facility, or a read primitive yields the
  entire layout instead of a single value.
- **Determinism covers the layout, not the canary.** Values remain fresh
  randomness rotating each round. Layout is deterministic and static;
  material is random and rotating.

The payoff is recoverability rather than reproducibility. Derived state can
be rebuilt from the seed at any time, so losing it is an inconvenience
rather than a catastrophe, and destroying it is not a winning move for
either party.
