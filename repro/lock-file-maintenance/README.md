# Lock file maintenance

`renovate.json` turns on lock file maintenance and lets it run at any time, so
Renovate opens a "Lock file maintenance" PR on its next run. Two APM projects
here have a lockfile:

- the root [`apm.yml`](../../apm.yml), which pins mattpocock/skills'
  `domain-modeling` skill to the tag `v1.0.0`
- [`apm.yml`](apm.yml) in this directory, which pins the same skill to the
  range `^1.0.0`, locked at `v1.0.0`. mattpocock/skills has since released
  `v1.2.3`, which adds `agents/openai.yaml` to the skill.

## What current Renovate does

For each project, it deletes `apm.lock.yaml` and then runs `apm install`. The
lockfile is APM's record of which deployed files it owns. Without it,
`apm install` treats the committed skill files as someone else's and leaves
them alone:

| Project | Lockfile after maintenance | Skill files |
|---|---|---|
| root | same pin, but every `deployed_files` and `deployed_file_hashes` entry and the whole `deployments` ledger are gone | unchanged |
| this directory | moved to `v1.2.3`, with the same entries gone | still `v1.0.0`: no `agents/openai.yaml` |

`apm audit` then reports drift in both projects: the deployed files as
unrecorded, and in this directory the two missing `agents/openai.yaml` files.
Compare Renovate's PR #1, which moves the root to `v1.2.3` with a normal
`apm install` and does add `agents/openai.yaml`.

## What the fix does

[MPV/renovate#6](https://github.com/MPV/renovate/pull/6) runs `apm update --yes`
instead, without deleting the lockfile. The root has nothing to move, so
nothing changes there. This directory moves to `v1.2.3`, gets
`agents/openai.yaml` in both `.claude/` and `.agents/`, and its ledger gains the
two new files. `apm.yml` isn't touched.
