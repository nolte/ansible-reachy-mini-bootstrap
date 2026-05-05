# ansible-reachy-mini-bootstrap

Ansible bootstrap for the Raspberry Pi inside the **Reachy Mini WiFi**
(Raspberry Pi OS, Bookworm-based, aarch64).

For installation, usage, and layout see the project [README](https://github.com/nolte/ansible-reachy-mini-bootstrap#readme).

## What it does

- Authorizes an SSH public key (from `pass`) on the `pollen` user
- Sets locale, timezone and a baseline package set
- Enables `unattended-upgrades` for automatic security patches
- Ships an `update` playbook for manual `apt full-upgrade`

## Quick reference

```bash
task              # list targets
task venv         # create dedicated .venv
task galaxy       # install Ansible collections
task ping         # connectivity check
task bootstrap    # initial setup
task update       # manual upgrade
```
