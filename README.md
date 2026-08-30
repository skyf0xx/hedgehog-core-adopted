# @skyf0xx/hedgehog-core-adopted

Hedgehog's adopted core: brings Hedgehog's discipline to a repo that
already exists, without bootstrapping a workspace at all. The build
graph covers new change only — pre-existing code is context to read and
respect, never a node in the graph.

Unlike other cores, this package ships no pre-built workspace and no
`hedgehog-core-design`-style architecture pass: the repo's stack, layout,
and commands are already decided, so this core wraps them rather than
choosing anything.

## Contents

- `agents/layer-eng.md` — builds each layer of an adopted core's change
  chain, one `hedgehog claim`ed packet at a time, gated by `hedgehog
  verify`.
- `skills/hedgehog-adopt` — brings Hedgehog's discipline to an existing
  repo: reads the repo read-only, proposes a linear-chain
  `.hedgehog/core.yaml` whose `verify` commands are the repo's own,
  confirmed with the user, and writes only `.hedgehog/`. Also the entry
  point for every later batch of change-work on an already-adopted repo.
- `skills/hedgehog-adopt-elicit` — a short clarifying pass for a large or
  under-specified change request on an already-adopted repo, run by
  `hedgehog-adopt` before adding that intent.
- `skills/hedgehog-authored-loop` — the operating loop for every unit of
  change-work on an adopted core: one layer per `hedgehog claim`ed
  packet via `layer-eng` (`hedgehog next` previews it read-only first),
  the Correction Protocol, and the per-change Stop Condition, all driven
  from `.hedgehog/core.yaml`.
- `CLAUDE.core.md` — fills a Hedgehog project's root `CLAUDE.md`
  `{{CORE_SECTION}}` placeholder for a repo Hedgehog adopted.
- `hedgehog-core.yaml` — this package's manifest: name, the selection
  prose the Hedgehog planner matches a project description against, and
  which agents/skills/templates it carries.

## Using this package

A Hedgehog installation depends on this package for the `adopted` core
rather than carrying its content directly. See the Hedgehog engine
(`@skyf0xx/hedgehog`) for the installer and build-graph tooling that
consumes it. Designing a workspace from scratch for a project that fits
no shipped core, rather than adopting Hedgehog onto one that already
exists, is a separate core,
[`@skyf0xx/hedgehog-core-authored`](https://github.com/skyf0xx/hedgehog-core-authored).

## Working on this core

This is a versioned npm package that the Hedgehog engine's `init` fetches
by name, carrying `adopted`'s own agent, skills, and the
`hedgehog-core.yaml` manifest that names them to the engine. This core
has no `init` step of its own — adoption's entry point is `hedgehog core
record-adopted`, which fetches this package and lands its agents/skills
onto an existing repo, invoked by `hedgehog-adopt` at first adoption. See
the engine repo ([`skyf0xx/hedgehog`](https://github.com/skyf0xx/hedgehog))
and its
[`ARCHITECTURE.md`](https://github.com/skyf0xx/hedgehog/blob/master/ARCHITECTURE.md)
for how that mechanism works — it lives there, not here.

No root `CLAUDE.md` lives in this repo. `CLAUDE.core.md` is a payload
file: its content is installed into a *consuming project's* generated
`CLAUDE.md`, filling that project's `{{CORE_SECTION}}` placeholder. A
plain root `CLAUDE.md` here would auto-load into any coding agent working
on this package itself, bleeding project-build context into a repo where
no Hedgehog build ever runs — build guidance for a project using this
core lives in that project's own generated `CLAUDE.md`, never here.

Changing this core means editing `agents/layer-eng.md` or one of the
three skills under `skills/` (`hedgehog-adopt`, `hedgehog-adopt-elicit`,
`hedgehog-authored-loop`). There is no `workspace/` template and no
regeneration script here — this core never scaffolds anything, since the
whole point is that the workspace already exists. A change here is a
release of this package, not of the engine: bump `package.json`'s
version, commit, and merge to `main` — this repo's own `publish.yml`
tags and publishes from there.
