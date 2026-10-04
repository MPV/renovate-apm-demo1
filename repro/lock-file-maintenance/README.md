# Lock file maintenance

This case uses the root [`apm.yml`](../../apm.yml) and
[`apm.lock.yaml`](../../apm.lock.yaml). `renovate.json` turns on lock file
maintenance and lets it run at any time, so Renovate opens a "Lock file
maintenance" PR on its next run.

## What current Renovate does

It deletes `apm.lock.yaml` and then runs `apm install`. `apm install` only
syncs the lockfile to the manifest; it never moves pins forward, so the delete
is the only reason the maintenance PR has a diff. The delete also throws away
what APM can't rebuild while the deployed files are already in the repository:
which files it owns, their hashes, and the ownership ledger.

With APM 0.31.0, this removed every `deployed_files` entry, every
`deployed_file_hashes` entry and the whole `deployments` ledger, and
`apm audit` then reported the deployed files as unrecorded
([MPV/renovate#12](https://github.com/MPV/renovate/issues/12)). The PR Renovate
opens here shows what happens with the APM version the Mend app runs. That was
APM 0.33.0 when it last updated this repository's lockfile (PR #1).

## What the fix does

[MPV/renovate#6](https://github.com/MPV/renovate/pull/6) runs `apm update --yes`
instead, without deleting the lockfile. It re-resolves the dependencies,
leaves `apm.yml` untouched, and rewrites the lockfile in place, keeping the
ledger.
