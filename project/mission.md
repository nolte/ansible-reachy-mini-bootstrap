---
mission_statement: "ansible-reachy-mini-bootstrap provisions a Reachy Mini WiFi device to a working Pollen/Reachy runtime state from a single inventory plus playbook run, so any owner reaches an identical, reproducible device state without manual per-device setup."
relevant_outcomes: [O-1, O-2]
audiences:
  - Reachy Mini owner / hobbyist
  - "`nolte` (repo author, first operator on his own device)"
verifies_via: F-1:acceptance-1
time_bound:
  kind: mvp_completion
mvp_status: achieved
created: 2026-07-02
revised_at: null
---

## Statement

`ansible-reachy-mini-bootstrap` provisions a Reachy Mini WiFi device to a working
Pollen/Reachy runtime state from a single inventory plus playbook run, so any
owner reaches an identical, reproducible device state without manual per-device
setup.

- **Specific** — the statement names *what* (Ansible provisioning of a Reachy
  Mini WiFi device) and *for whom* (Reachy Mini owners and the author-operator,
  resolved in `audiences`).
- **Measurable** — `verifies_via: F-1:acceptance-1`: running the playbook against
  a fresh device brings it to a working Pollen/Reachy runtime state.
- **Achievable** — the minimum viable product is the shipped
  `reachy-mini-ansible-provisioning` capability; roadmap item R-1 is `mvp: true`,
  `detail: fine`, `target_sprint: 1`.
- **Relevant** — `relevant_outcomes: [O-1, O-2]`, each resolving to an outcome in
  `project/goals.md`.
- **Time-bound** — `time_bound: { kind: mvp_completion }`; the bound is the moment
  the shipped MVP is recorded as achieved.

## Audiences

- **Reachy Mini owner / hobbyist** — the MVP delivers an Ansible playbook and
  role collection that a owner runs against their Reachy Mini WiFi device to
  reach a working Pollen/Reachy runtime, with no manual per-device setup.
- **`nolte` (repo author, first operator on his own device)** — the MVP delivers
  a reproducible provisioning run the author applies to his own device, reaching
  an identical state each time from source.

## Verification

The mission is verified by feature **F-1 — Provisioned device**, acceptance
criterion 1: *"Running the playbook against a fresh Reachy Mini WiFi device
brings it to a working state, with network, system dependencies, and the
Pollen/Reachy runtime in place."* This is the `verifies_sprint_value` criterion
for sprint 0001 and holds against the shipped playbook, so the MVP is recorded
as `achieved`.

## Source

- **Audience artefact**: `AUDIENCES.md` at the `ansible-reachy-mini-bootstrap`
  repository root (consulted at its current develop tip); the two `audiences`
  entries are the device owner and the author-operator.
- **Outcomes referenced**: O-1, O-2 from `project/goals.md`.
- **Authored by**: the `mission-define` cascade (issue nolte/claude-shared#262
  mission-authoring backfill), 2026-07-02. The MVP is modelled retroactively: the
  provisioning capability was already `status: active` when the repository
  adopted the planning suite, so R-1 is recorded `status: done` and `mvp_status`
  opens at `achieved`.
