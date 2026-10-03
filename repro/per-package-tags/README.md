# Per-package tags

A monorepo that versions each package separately tags each release with the
package's name. The [fixture monorepo](../../fixtures/monorepo/) does that for
three skills, in both common forms: `<name>--v<version>` (Claude Code's
`claude plugin tag`) and `<name>-v<version>` (release-please). APM resolves
both. This `apm.yml` pins each skill to its first release.

## What current Renovate does

No updates for any of the four entries, and no warning or skip reason, so they
look up to date.

The default versioning (`semver-coerced`) accepts `code-review--v1.0.0` as a
version but counts every prefixed tag as unstable. With `ignoreUnstable`, every
newer tag is dropped.

## What the fix does

[MPV/renovate#14](https://github.com/MPV/renovate/pull/14) compares each
dependency only with the tags of its own package:

| Entry | Update |
|---|---|
| `…/code-review#code-review--v1.0.0` | `code-review--v1.1.0` |
| `…/triage#triage--v1.0.0` | `triage--v2.0.0` |
| `…/release-notes#release-notes-v1.0.0` | `release-notes-v1.4.0` |
| `…/code-review#<sha> # code-review--v1.0.0` | `code-review--v1.1.0`, with that tag's commit |

The fixture has no repository-wide `v<version>` tags. In a monorepo that also
has them, current Renovate instead proposes the newest repository-wide tag for
every package.
[MPV/renovate#15](https://github.com/MPV/renovate/issues/15) drafts the general
question for Renovate's lookup.
