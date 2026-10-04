# Semver range refs

APM accepts a semver range as the ref and installs the highest tag that
satisfies it. [mattpocock/skills](https://github.com/mattpocock/skills) tags its
releases `v<version>`. The Dependency Dashboard here shows `v1.2.3` as its
newest.

The entries pin the repository itself rather than the skill the root `apm.yml`
uses, so Renovate's update for them gets its own branch instead of joining the
root's.

## What current Renovate does

When a range no longer covers the newest tag, it replaces the range with that
tag, so all three entries become `#v1.2.3` in one PR. The ranges are gone, and
in a project with a lockfile APM stops recording a `constraint` for them. The
`~1.0` entry, which APM rejects, gets the same treatment instead of being
flagged.

## What the fix does

[MPV/renovate#16](https://github.com/MPV/renovate/pull/16) keeps APM ranges as
ranges:

| Entry | Update |
|---|---|
| `#~1.0.0` | `#~1.2.0` |
| `#1.0.x` | `#1.2.x` |
| `#~1.0` | skipped as `invalid-version`, as APM rejects it |

`apm install` in this directory fails on purpose, on the `~1.0` entry.
