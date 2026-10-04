# Clone URL and SSH entries

APM accepts a clone URL followed by `#<ref>` as well as the `owner/repo`
shorthand. This `apm.yml` uses the three URL forms, all for APM's own
repository, [microsoft/apm](https://github.com/microsoft/apm), at `v0.30.0`. A
real manifest would use just one of them.

## What current Renovate does

It splits each entry on `/` as if it were shorthand:

| Entry | Looked up as |
|---|---|
| `https://github.com/microsoft/apm.git#v0.30.0` | `github-tags` package `https:/github.com`, so the lookup fails |
| `git@github.com:microsoft/apm.git#v0.30.0` | skipped as `invalid-dependency-specification` |
| `ssh://git@github.com/microsoft/apm.git#v0.30.0` | `github-tags` package `ssh:/git@github.com`, so the lookup fails |

## What the fix does

[MPV/renovate#17](https://github.com/MPV/renovate/pull/17) parses clone URLs and
looks them up the same way as the shorthand: all three become `github-tags`
package `microsoft/apm`, with an update to the newest release (`v0.33.0` at the
time of writing).
