---
number: 1
status: closed
started: 2026-07-02
ended: 2026-07-02
value_statement: A Reachy Mini WiFi owner runs one playbook and reaches a working device state, with network, system dependencies, and the Pollen/Reachy runtime in place.
artifact_ref: develop (shipped capability, pre-planning-suite)
roadmap_items: [R-1]
features: [F-1]
---

## Goal

A Reachy Mini WiFi owner provisions a device to a working state from a single
inventory plus playbook run. Success is verified by F-1 `acceptance-1`: the
playbook brings a fresh device to a working Pollen/Reachy runtime state.

## Features

- [F-1](../features/provisioned-device.md) — Provisioned device — status: done

## Out of scope

- The Reachy Mini hardware and the Pollen/Reachy upstream runtime themselves.
- Reachy Mini behaviour applications (a separate repository).

## Review notes

Retroactive reconciliation (2026-07-02): the `reachy-mini-ansible-provisioning`
capability was already `status: active` before this repository adopted the
planning suite (issue nolte/claude-shared#262 mission-authoring backfill). This
sprint records roadmap item R-1 and feature F-1 as `done`, and itself as
`closed`, to document the delivered MVP rather than to plan new work.
