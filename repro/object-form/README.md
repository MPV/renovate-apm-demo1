# Object-form entries

APM accepts a dependency as an object as well as a string. This `apm.yml` uses
only the object form: a git repository with `path` and `ref`, a marketplace
plugin, and a local directory.

## What current Renovate does

It drops object entries silently. With no string entries, this file doesn't
appear on the Dependency Dashboard at all, so the outdated `ref: v1.0.0` looks
handled.

## What the fixes do

- [MPV/renovate#13](https://github.com/MPV/renovate/pull/13) reports every
  object entry with a skip reason:

  | Entry | Skip reason |
  |---|---|
  | `git` | `unsupported` |
  | `marketplace` | `unknown-registry`, since the marketplace is registered on the installing machine |
  | `path` | `local-dependency` |

- [MPV/renovate#18](https://github.com/MPV/renovate/pull/18), on top of it,
  updates the `git` entry by rewriting its `ref:` line, from `v1.0.0` to
  `v1.2.3`, without touching its other keys.

`apm install` in this directory needs `some-marketplace` registered first.
