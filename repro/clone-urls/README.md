# Clone URL and SSH entries

APM accepts a clone URL followed by `#<ref>` as well as the `owner/repo`
shorthand. This `apm.yml` uses the three URL forms, all for
[mattpocock/skills](https://github.com/mattpocock/skills) at `v1.0.0`. A real
manifest would use just one of them.

## What current Renovate does

It splits each entry on `/` as if it were shorthand:

| Entry | Looked up as |
|---|---|
| `https://github.com/mattpocock/skills.git#v1.0.0` | `github-tags` package `https:/github.com`, so the lookup fails |
| `git@github.com:mattpocock/skills.git#v1.0.0` | skipped as `invalid-dependency-specification` |
| `ssh://git@github.com/mattpocock/skills.git#v1.0.0` | `github-tags` package `ssh:/git@github.com`, so the lookup fails |

## What the fix does

[MPV/renovate#17](https://github.com/MPV/renovate/pull/17) parses clone URLs and
looks them up the same way as the shorthand: all three become `github-tags`
package `mattpocock/skills`, with an update to `v1.2.3`.
