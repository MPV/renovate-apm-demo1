# Per-package tags

A monorepo that versions each package separately tags each release with the
package's name. [laurigates/claude-plugins](https://github.com/laurigates/claude-plugins)
is one: a Claude Code plugin marketplace that releases each of its plugins with
release-please, tagged `<plugin>-v<version>`, such as `documentation-plugin-v1.13.0`
or `configure-plugin-v1.36.2`. APM resolves this form. This `apm.yml` pins three
of its plugins to older releases.

## What current Renovate does

No updates for any of the three entries, and no warning or skip reason, so they
look up to date.

The default versioning (`semver-coerced`) accepts `documentation-plugin-v1.12.0`
as a version but counts every prefixed tag as unstable. With `ignoreUnstable`,
every newer tag is dropped.

## What the fix does

[MPV/renovate#14](https://github.com/MPV/renovate/pull/14) compares each
dependency only with the tags of its own package:

| Entry | Update |
|---|---|
| `…/documentation-plugin#documentation-plugin-v1.12.0` | the newest `documentation-plugin-v…`, at least `v1.13.0` |
| `…/configure-plugin#configure-plugin-v1.35.1` | the newest `configure-plugin-v…`, at least `v1.36.2` |
| `…/project-plugin#project-plugin-v1.21.7` | the newest `project-plugin-v…`, at least `v1.21.8` |

If the repository also had a repository-wide `v<version>` tag newer than a
plugin's version, current Renovate would propose that tag for the plugin
instead of nothing.
[MPV/renovate#15](https://github.com/MPV/renovate/issues/15) drafts the general
question for Renovate's lookup.
