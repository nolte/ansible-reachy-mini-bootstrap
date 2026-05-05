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

## First run

`task bootstrap` already passes `--ask-pass` so the SSH password for
`pollen` is prompted on the very first connect (before the SSH key is
authorized). On a fresh device you usually also need the sudo password
for privilege escalation — pass `--ask-become-pass` through to
`ansible-playbook` via Task's `CLI_ARGS` mechanism (everything after `--`):

```bash
task bootstrap -- --ask-become-pass        # also prompt for the sudo password
task bootstrap -- -vvv                     # verbose run
task bootstrap -- --limit reachy-mini -C   # check-mode on a single host
```

The same `--`-passthrough works for `task update` and `task ping`.
