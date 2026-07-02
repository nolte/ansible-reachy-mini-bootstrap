---
id: F-1
title: Provisioned device
status: done
roadmap_item: R-1
sprint: 1
created: 2026-07-02
ended: 2026-07-02
verifies_sprint_value: acceptance-1
consistency_check:
  performed_at: 2026-07-02
  agent_version: manual-fallback (retroactive; feature-consistency-reviewer not run cross-repo)
  findings:
    - kind: clean
      target: project/features/
      resolution: proceed
      evidence: "project/features/ empty (first decomposition); no feature-to-feature overlap possible."
    - kind: prior-art
      target: the shipped Ansible playbook and roles
      resolution: proceed
      evidence: "The provisioning playbook and roles already exist; F-1 documents the provisioning contract, it does not build new automation."
---

## Description

F-1 is the mission-verifying feature for the shipped
`reachy-mini-ansible-provisioning` capability. The Ansible playbook and role
collection provision a Reachy Mini WiFi device from a single inventory plus
playbook run. The contract is met when running the playbook against a fresh
device brings it to a working Pollen/Reachy runtime state. This holds against the
shipped playbook, so the feature is recorded `done` as part of the retroactive
MVP reconciliation (issue nolte/claude-shared#262).

## Acceptance criteria

- [x] **acceptance-1** Running the playbook against a fresh Reachy Mini WiFi
  device brings it to a working state, with network, system dependencies, and the
  Pollen/Reachy runtime in place. _(This is the sprint value verifier.)_
- [x] **acceptance-2** The run is driven by a single inventory plus playbook, with
  no manual per-device steps.
- [x] **acceptance-3** Re-running the playbook reaches an identical device state
  (idempotent provisioning).

## Test hooks

- **acceptance-1** — a provisioning run against a device or test image — passing.
- **acceptance-2** — inspect the inventory and playbook entry point — passing.
- **acceptance-3** — a second playbook run reports no changes — passing.

## Consistency notes

Retroactive documentation feature: the Ansible playbook predates the planning
suite. No new implementation is introduced; the feature exists so the mission's
`verifies_via: F-1:acceptance-1` and sprint 1's `value_statement` resolve to a
real acceptance criterion.

## References

- `project/portfolio.yml` capability `reachy-mini-ansible-provisioning`
- `AUDIENCES.md` audience "Reachy Mini owner / hobbyist"
- `README.md` (the provisioning playbook and inventory)
