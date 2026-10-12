# Reproductions for Renovate's `apm` manager

Each directory here holds an `apm.yml` that shows one behaviour of Renovate's
[`apm` manager](https://docs.renovatebot.com/modules/manager/apm/), with a
README saying what current Renovate does and what the proposed fix changes.

| Case | What it shows | Proposed fix |
|---|---|---|
| [`per-package-tags`](per-package-tags/) | Dependencies pinned to per-package tags (`documentation-plugin-v1.12.0`) get no updates. | [MPV/renovate#14](https://github.com/MPV/renovate/pull/14), [MPV/renovate#15](https://github.com/MPV/renovate/issues/15) |
| [`semver-ranges`](semver-ranges/) | Semver range refs (`#~0.30.0`) are replaced by a literal tag. | [MPV/renovate#16](https://github.com/MPV/renovate/pull/16) |
| [`object-form`](object-form/) | Object-form entries (`git:` with `ref:`) are dropped. | [MPV/renovate#13](https://github.com/MPV/renovate/pull/13), [MPV/renovate#18](https://github.com/MPV/renovate/pull/18) |
| [`clone-urls`](clone-urls/) | Clone URL and SSH entries are looked up under a broken package name, or skipped. | [MPV/renovate#17](https://github.com/MPV/renovate/pull/17) |
| [`lock-file-maintenance`](lock-file-maintenance/) | Lock file maintenance deletes `apm.lock.yaml` before running `apm install`, which loses APM's record of the deployed files and, when a ref moves, leaves them at the old version. | [MPV/renovate#6](https://github.com/MPV/renovate/pull/6) |

The cases that need a repository tagged `v<version>` use APM's own,
[microsoft/apm](https://github.com/microsoft/apm). None of the bugs is specific
to it, or to mattpocock/skills, the root `apm.yml`'s dependency, which only the
lock file maintenance case uses.

## How Renovate runs here

The Mend Renovate app runs the latest Renovate release on `master` and reports
on the [Dependency Dashboard](https://github.com/MPV/renovate-apm-demo1/issues/2).
Each `apm.yml` is its own package file there.

Each case is its own PR and can be merged on its own, in any order. The cases
use dependency names that differ from each other and from the root `apm.yml`,
so Renovate keeps their updates on separate branches without any extra
configuration.

Renovate's PRs for these directories are part of the reproduction. Close them
rather than merging them, or the reproduction is gone.

## Running Renovate yourself

From a checkout of this repository, with a GitHub token for the tag lookups:

```sh
GITHUB_COM_TOKEN=<token> LOG_LEVEL=debug npx renovate --platform=local --dry-run=lookup
```

To try a fix, run Renovate from a checkout of the fix's branch in
[MPV/renovate](https://github.com/MPV/renovate) instead, still from this
repository's directory:

```sh
GITHUB_COM_TOKEN=<token> LOG_LEVEL=debug node <path-to-renovate>/lib/renovate.ts --platform=local --dry-run=lookup
```
