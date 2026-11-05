# .entrain

This organization's Entrain repository: the rules Entrain reviews its pull
requests under, and the agent context (skills and instructions) its sandboxes
load. It is flat: its root is the organization's scope and `projects/<key>/`
each project's.

- `rules/` (and `attributes/`, `review/`) at the root holds the organization's
  rules, which apply to every pull request in its repositories.
- `projects/<key>/rules/` holds a project's rules, which apply to the pull
  requests its sessions contribute to in its repositories.
- `.entrain/rules/` holds this repository's own rules, like any repository's:
  they apply to pull requests in this repository alone.
- `AGENTS.md`, `skills/` and `prompts/` at each scope carry the agent context
  loaded in sandboxes.

Migrating from the earlier nested layout: move the organization's
`.entrain/rules/` to `rules/` (and the other scope directories alike), and each
`projects/<key>/.entrain/rules/` to `projects/<key>/rules/`. Leave a repository
scope only in `.entrain/` if this repository needs rules of its own.

The organization repository is flat so that its own `.entrain/` can be a
repository scope like any other repository's
([org trees in sandboxes](https://github.com/EntrainAI/entrain-src/blob/main/docs/working/org-trees-in-sandboxes.md), decision 4).

Changing a rule is a pull request here, reviewed under these rules.
`rule-changes` is locked: a change to a rule, an attribute, `.gitattributes` or
`CODEOWNERS` needs a person's approval, the author's included. A push dismisses
it (`dismiss: on-push`): an approval counts only at the head it approved, or at
one that makes exactly the same changes, as after a rebase onto other files.
It applies in every repository, so it covers only `.entrain/` there; this
repository's own `scope-changes` asks the same for the organization's and
projects' directories here.

Keep the default branch protected, requiring a pull request and the
`entrain/rules` check, so that no direct push changes every repository's rules
unreviewed.
