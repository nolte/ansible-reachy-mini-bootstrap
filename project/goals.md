# Vision

`ansible-reachy-mini-bootstrap` is the nolte portfolio's device-provisioning
automation for the Reachy Mini WiFi (Raspberry Pi OS). An Ansible playbook and
role collection bring a device to a working state — network, system
dependencies, and the Pollen/Reachy runtime — from a single inventory plus
playbook run. The Ansible source lives here so any owner reaches an identical
device state from one run.

## Outcomes

- **O-1** — a Reachy Mini WiFi device reaches a working state (network, system
  dependencies, and the Pollen/Reachy runtime) from one inventory plus playbook
  run, without manual per-device setup. _(audience: Reachy Mini owner / hobbyist)_
- **O-2** — the author reprovisions his own device reproducibly from source,
  reaching an identical state each time. _(audience: `nolte` (repo author, first operator on his own device))_
