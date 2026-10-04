# Semver range refs

APM accepts a semver range as the ref and installs the highest tag that
satisfies it. This `apm.yml` pins APM's own `apm-guide` skill package, from
[microsoft/apm](https://github.com/microsoft/apm), with ranges that stop at
`0.30`. APM tags its releases `v<version>`, and its newest is past that range:
`v0.33.0` at the time of writing.

## What current Renovate does

When a range no longer covers the newest tag, it replaces the range with that
tag, so all three entries become `#v0.33.0` (or whichever release is newest)
in one PR. The ranges are gone, and in a project with a lockfile APM stops
recording a `constraint` for them. The `~0.30` entry, which APM rejects, gets
the same treatment instead of being flagged.

## What the fix does

[MPV/renovate#16](https://github.com/MPV/renovate/pull/16) keeps APM ranges as
ranges, moved up to the newest release:

| Entry | Update, with `v0.33.0` the newest |
|---|---|
| `#~0.30.0` | `#~0.33.0` |
| `#0.30.x` | `#0.33.x` |
| `#~0.30` | skipped as `invalid-version`, as APM rejects it |

`apm install` in this directory fails on purpose, on the `~0.30` entry.
