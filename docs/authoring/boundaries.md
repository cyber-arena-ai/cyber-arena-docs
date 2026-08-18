# Inherited boundaries

## Boundaries are inherited, not authored

The infrastructure rule constrains the attacker: a capture may not be scored
through challenge infrastructure. The same principle constrains the author,
and the second half is easier to miss. Where the layer model says how a
sound cell is built, this section says what invalidates one. The contest is against the application, so
the application must be both what is attacked and what defends.

An application decomposes into three parts for this purpose. Its **runtime**
— code and authorization logic — is inherited from upstream. Its
**configuration** — roles, permissions, visibility, ACLs — is written by the
author, but in the application's own vocabulary. Its **data** — identities,
records, secrets, artifacts — is written by the author outright.

Authoring the data is expected; every challenge seeds its own. What
invalidates a cell is authoring the *protection*:

> Provisioning may create assets. It must not create the protection that
> makes an asset a cell. If the challenge package is what stops the baseline
> identity, the cell measures the package rather than the application.

Establishment is the test, not the enforcement mechanism. An application
that creates its own secrets directory restricted to its own account has
established that boundary even though the operating system enforces it, and
a cell behind it is sound. A directory the challenge restricts itself,
overriding the mode the application chose, is authored, and a cell behind it
is not — defeating it corresponds to no capability against any real
deployment of that software.

Packaging routinely collapses separations the application relies on: a
component that upstream runs on its own host arrives as another account in
one container. The tempting move is to rebuild the lost separation and call
it restoration rather than authoring. That distinction does not survive
contact with the two cases it covers, which are not alike.

Where the separation is expressible in the **application's own
configuration** — a grant, a role, an access control list, a visibility
setting — nothing was lost and nothing needs restoring. Configuration is
written by the author but in the application's vocabulary, so the boundary
is established by the application wherever it runs. A database role denied
access to a table is the same boundary whether the database sits in its own
container or beside the service.

Where the separation exists **only as deployment topology** — separate
hosts, separate accounts, network isolation — there is nothing to inherit.
In the configuration actually deployed the application establishes no
boundary at that point, so no cell exists there. Adding one means the
challenge inventing protection, and an attacker defeating it defeats the
challenge's own file modes rather than anything true of that software
elsewhere.

The tell is mechanical, and both authors and reviewers should use it:
**protection a cell depends on that appears in the challenge's provisioning
and nowhere upstream is authored.** A `chmod`, `chown`, `setfacl`, or
tightened `umask` touching a cell's asset is the signature.

Assets an application protects by topology alone therefore yield no cells
under one-container packaging. That is a stated limitation rather than a
defect to work around: the contest is against the application, and the
packaging is ours.

Capability implication is separate from this and is not a defect. A stronger
outcome will usually subsume a weaker one, so an attacker who reaches an
execution cell may collect a read cell in the same step. That is the shape
of a ladder, not a flaw in it. What independence requires is that no cell's
witness is contained in or derived from another's — distinct planted values
in distinct locations — so that when a defender closes one path the
remaining cells still have to be reached on their own.

## Genuine and arena-specific paths

Because any genuine vulnerability may score, whether the agent used the
originally selected vulnerability cannot be the cheat test. The distinction
that matters is whether a successful path crosses a boundary that exists in
the upstream application or one that exists only because the arena built
the box.

The asset graph is the written referent for that judgment. A path is
genuine when its crossed boundary appears in the graph and affects broader
upstream use, and arena-specific when it depends on fixtures, secrets,
interfaces, or behavior introduced by the challenge environment. Recording
the graph during curation therefore serves both cell placement and later
cheat review.

Canary evidence must stay outside the agent's reach and outside the mutable
application. Values, their locations, and the scoring path should not be
readable, forgeable, or replayable from anything the attacker or the
defender controls, and no capability should be creditable from a single
memorized constant.
