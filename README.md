# .entrain

This organization's Entrain repository: the gates Entrain reviews its pull
requests under, and the agent context (skills and instructions) its sandboxes
load. It is flat: its root is the organization's scope and `projects/<key>/`
each project's.

- `gates/` (and `attributes/`, `review/`) at the root holds the organization's
  gates, which apply to pull requests into the default branch in its repositories.
- `classifiers/` at the root holds the organization's classifiers: the
  questions a decision model answers, such as `finding-kind.yaml`, the kinds
  Entrain tags each agent finding with for review analytics. Edit its kinds
  to fit your organization. A classifier is asked with the model its file
  names (`model: <provider>/<model-id>`), else the organization's default
  classifier model in Settings › Preferences; with neither, nothing is asked.
- `projects/<key>/gates/` holds a project's gates, which apply to the pull
  requests its sessions contribute to in its repositories, on the default branch.
- `.entrain/gates/` holds this repository's own gates, like any repository's:
  they apply to pull requests into the default branch in this repository alone.
- `AGENTS.md`, `skills/` and `prompts/` at each scope carry the agent context
  loaded in sandboxes.

Every scope with gates or attributes needs a `spec.yaml` containing exactly
`version: entrain.dev/v1alpha2`: at the root, in each `projects/<key>/`, and in
`.entrain/`. This selects v1alpha2, whose gates live in `gates/`. A scope still on
`entrain.dev/v1alpha1` keeps `rules/` and is read with a deprecation warning until
it moves. Gates use `action: merge` and gate
the default branch unless they list other `branches` explicitly.

Migrating from v1alpha1: set the scope's `spec.yaml` to
`version: entrain.dev/v1alpha2`, move `rules/` to `gates/`, and rewrite selectors
in v1alpha2's names: `scope: fileChange`, and `sides.exists(…)`/`sides.all(…)`
for a file change's paths and attributes. The checker names each v1alpha1 name
and what to write instead.

Migrating from the earlier nested layout: move the organization's
`.entrain/gates/` to `gates/` (and the other scope directories alike), and each
`projects/<key>/.entrain/gates/` to `projects/<key>/gates/`. Leave a repository
scope only in `.entrain/` if this repository needs gates of its own.

The organization repository is flat so that its own `.entrain/` can be a
repository scope like any other repository's
([org trees in sandboxes](https://github.com/EntrainAI/entrain-src/blob/main/docs/working/org-trees-in-sandboxes.md), decision 4).

Changing a gate is a pull request here, reviewed under these gates.
`gate-changes` is locked: a change to a gate or rule, an attribute, `spec.yaml`,
`.gitattributes` or `CODEOWNERS` needs a person's approval, the author's included.
A push dismisses it (`dismiss: on-push`): an approval counts only at the head it approved, or at
one that makes exactly the same changes, as after a rebase onto other files.
It applies in every repository, covering both `.entrain/gates/` and a tree not
yet moved from `.entrain/rules/`, along with the other protected files; this
repository's own `shared-gate-changes` asks the same for the organization's and
projects' directories here.

Keep the default branch protected, requiring a pull request and the
`entrain/gates` check, so that no direct push changes every repository's gates
unreviewed.
