# Establishing the asset graph


The asset graph is not reliably extractable by mechanical analysis. An
application's principals, protected assets, and enforced boundaries are
expressed differently across frameworks, languages, and configuration
styles, and the boundary that matters is often a convention rather than a
construct a tool can name.

The graph is therefore a semantic reviewer's inference during static
triage, recorded as an explicit hypothesis with the evidence behind it, and
confirmed at runtime before authoring begins. Runtime confirmation
establishes three things for the proposed objectives: that each has a real
plantable location in the running service, that the baseline identity
cannot reach each canary through the legitimate workflow, and that an objective
excluded on paper is genuinely absent rather than merely unexamined.

The two steps have opposite cost profiles, and the process should exploit
that. Inference is cheap and sometimes wrong, so it should be broad;
confirmation is expensive and decisive, so it should prune. Authoring
inherits only confirmed objectives, and an objective that cannot be confirmed is
dropped or carried forward as a stated unknown rather than silently built.
