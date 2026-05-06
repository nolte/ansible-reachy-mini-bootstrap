# CLAUDE.md

Hints for Claude / AI agents working on this repository.

## What this repo is

Ansible bootstrap for the Reachy Mini WiFi onboard Raspberry Pi
(Raspberry Pi OS, Bookworm-based, aarch64). It configures the
device's `pollen` user with an authorized SSH key, sets locale and
baseline packages, and enables `unattended-upgrades` for automatic
security patches. A second playbook ships manual full-upgrades.

## Entry points

All developer commands run through Task (`Taskfile.yml`):

- `task venv` — create the dedicated `.venv/` and install Ansible
- `task galaxy` — install Ansible collections from `requirements.yml`
- `task ping` — connectivity check against the `reachy_mini` group
- `task bootstrap` — one-time setup playbook (`playbooks/site.yml`)
- `task update` — manual `apt full-upgrade` (`playbooks/update.yml`)
- `task lint` — `ansible-lint` over `playbooks/`
- `task test` — syntax-check + lint (Molecule comes later)
- `task docs` — `mkdocs build` smoke build

`task bootstrap` and `task update` use `--ask-become-pass`; the
`pollen` sudo password is requested once per run.

## Layout (Ansible repository convention)

```
ansible.cfg                          # collections_path, defaults
inventory/hosts.yml                  # target host(s) + per-host overrides
inventory/group_vars/reachy_mini.yml # shared variables (next to the inventory!)
playbooks/                           # site.yml, update.yml
roles/                               # common, user, unattended_upgrades
requirements.yml                     # ansible-galaxy collections
requirements.txt                     # python (ansible, ansible-lint)
```

The repository deliberately uses the Ansible standard top-level
layout — wrapping it under `src/` would break `ansible-playbook`'s
default role and inventory discovery. This is documented as an
exception in `nolte/claude-shared:spec/project/project-structure/`.

## Repository profile

This repo declares itself as **`single-environment-bootstrap`** per
`nolte/claude-shared:spec/ansible/playbook-development/` — exactly one
target device (`reachy-mini.local`), no `<env>` segment in the
inventory, and inline `roles/<name>/` directories that hold
device-specific configuration not consumed by any other repository.
The moment a second repository would consume one of these roles, that
role MUST be extracted into its own role repo per
`spec/ansible/role-development/` and pulled in via `requirements.yml`.

## Conventions Claude must respect

- **SSH public key comes from `pass`**, not from a tracked file.
  `inventory/hosts.yml` resolves it via
  `lookup('pipe', 'pass show private/keyfiles/ssh/private_ed25519/id_ed25519.pub')`
  (sits next to `target_user` and `timezone` so it's per-host overridable).
  Don't move the key into the repo or `.env`.
- **`group_vars/` lives next to the inventory** (`inventory/group_vars/`),
  not at the repo root. Top-level `group_vars/` is only picked up when
  `playbook_dir` equals the repo root, which is not the case here
  because playbooks live under `playbooks/`.
- **No silent password disablement.** SSH password auth is left
  enabled so the operator can't lock themselves out — flip it via a
  variable, not a default.
- **Reboots are operator-controlled.** `unattended_upgrades_automatic_reboot`
  defaults to `false`; do not change without asking.
- **Roles stay in `roles/<name>/{tasks,handlers,templates}`.**
  Don't restructure into a flat layout.
- **Pin reusable workflows to release tags** (`@v1.1.15` etc.),
  not to moving branches.
