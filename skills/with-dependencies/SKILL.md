---
name: with-dependencies
description: A test skill with an APM dependency of its own, for reproducing Renovate's handling of the apm.yml copies that APM deploys.
---

# With dependencies

This skill only exists to test Renovate. Its `apm.yml` makes APM install mattpocock's `grill-me` skill alongside it, and APM copies that `apm.yml` into each project that installs this skill.
