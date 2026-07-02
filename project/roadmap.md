# Roadmap

This file is the work queue governed by `spec/project/roadmap/`. Each entry is a
level-3 heading followed by a `yaml` code block (`id`, `title`, `detail`,
`outcomes`, `target_sprint`, `mvp`, `status`, in that order) and a free-text
body. `roadmap-plan` and `roadmap-refine` own the detail level and the status
lifecycle; do not hand-edit those fields here.

Entries carry monotonically increasing IDs starting at `R-1`, never reused.
Outcome IDs (`O-n` in `goals.md`) are an independent counter.

`ansible-reachy-mini-bootstrap` shipped the MVP item below before adopting the
planning suite. This roadmap records it retroactively as `status: done`, mapped
to sprint 1, so the mission's minimum viable product resolves.

## Phase 1 — Device provisioning

### R-1 — Reachy Mini WiFi provisioning playbook

```yaml
id: R-1
title: Reachy Mini WiFi provisioning playbook
detail: fine
outcomes: [O-1, O-2]
target_sprint: 1
mvp: true
status: done
```

The Ansible playbook and role collection that provisions a Reachy Mini WiFi
device (Raspberry Pi OS): network, system dependencies, and the Pollen/Reachy
runtime, from a single inventory plus playbook run. Capability
`reachy-mini-ansible-provisioning` in `project/portfolio.yml`.
