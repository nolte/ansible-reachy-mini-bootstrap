# ansible-reachy-mini-bootstrap

[![CI](https://github.com/nolte/ansible-reachy-mini-bootstrap/actions/workflows/ci.yml/badge.svg)](https://github.com/nolte/ansible-reachy-mini-bootstrap/actions/workflows/ci.yml)
[![Release Drafter](https://github.com/nolte/ansible-reachy-mini-bootstrap/actions/workflows/release-drafter.yml/badge.svg)](https://github.com/nolte/ansible-reachy-mini-bootstrap/actions/workflows/release-drafter.yml)

Ansible-Bootstrap für den Raspberry Pi des **Reachy Mini WiFi**
(Raspberry Pi OS, Bookworm-basiert, aarch64).

Stellt sicher, dass auf dem Gerät:

- der Standard-User `pollen` einen autorisierten SSH-Key hat,
- Zeitzone und Baseline-Pakete gesetzt sind,
- `unattended-upgrades` automatische Sicherheitsupdates einspielt,
- ein einfacher Befehl manuelle Voll-Updates ausrollt.

## Voraussetzungen auf dem Control-Host

- `python3` mit `venv`-Modul
- [Task](https://taskfile.dev) (`task` v3+)
- `pass` (passwordstore.org), inklusive entsperrter GPG-Identität
- Erreichbarkeit von `reachy-mini.local` per SSH als User `pollen`
  (initial mit Passwort)

Der SSH-Public-Key wird zur Laufzeit aus dem Passwordstore gelesen:

```
pass show private/keyfiles/ssh/private_ed25519/id_ed25519.pub
```

## Verwendung

```bash
task              # listet alle verfügbaren Targets
task venv         # legt .venv/ an, installiert ansible + ansible-lint
task galaxy       # installiert Ansible-Collections aus requirements.yml
task ping         # connectivity-check (Modul: ping)
task bootstrap    # einmaliges Setup (User, SSH-Key, Updates-Policy, Baseline)
task update       # apt full-upgrade + Reboot-Hinweis
task lint         # ansible-lint über playbooks/
```

`task bootstrap` und `task update` fragen mit `--ask-become-pass`
nach dem sudo-Passwort von `pollen`. Zusätzliche Ansible-Argumente
können über `--` weitergereicht werden, z.B.:

```bash
task bootstrap -- --check --diff
task update    -- --limit reachy-mini
```

## Layout

```
ansible.cfg
inventory/hosts.yml                  # Zielhost: reachy-mini.local, user pollen
inventory/group_vars/reachy_mini.yml # Variablen (baseline_packages, upgrade-policy)
playbooks/
  site.yml                           # Bootstrap
  update.yml                         # Voll-Update auf Abruf
roles/
  common/                            # apt-baseline + timezone
  user/                              # pollen + authorized_keys
  unattended_upgrades/               # automatische Updates
requirements.txt                     # Python: ansible, ansible-lint
requirements.yml                     # Ansible-Collections
Taskfile.yml                         # venv + Targets
```

## Inventory anpassen

Falls `reachy-mini.local` per mDNS nicht aufgelöst wird, in
`inventory/hosts.yml` `ansible_host` auf die IP setzen.
