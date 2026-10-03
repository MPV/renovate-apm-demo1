# Fixture monorepo

Three skills that are versioned and tagged separately, the way a monorepo of
skills or plugins tags its releases:

| Skill | Tag form | Tags |
|---|---|---|
| `code-review` | `<name>--v<version>`, as `claude plugin tag` creates | `code-review--v1.0.0`, `code-review--v1.1.0` |
| `triage` | `<name>--v<version>` | `triage--v1.0.0`, `triage--v2.0.0` |
| `release-notes` | `<name>-v<version>`, as release-please creates | `release-notes-v1.0.0`, `release-notes-v1.4.0` |

The repository has no repository-wide `v<version>` tags. Each tag points at the
commit that set that skill's `metadata.version`.

Used by [`repro/per-package-tags`](../../repro/per-package-tags/).
