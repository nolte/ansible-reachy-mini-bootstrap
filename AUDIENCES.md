# Audiences — `nolte/ansible-reachy-mini-bootstrap` (this repository)

<!--
Produced via the `audience-identify` skill, following
spec/project/audience-identification/.
Do not add audiences without first declaring the bounded context below.
-->

## Bounded context

**What this context *is***:

- The repository `nolte/ansible-reachy-mini-bootstrap` — an Ansible automation project that bootstraps and maintains the onboard Raspberry Pi inside a Reachy Mini WiFi (Raspberry Pi OS, Bookworm-based, aarch64).
- Ships two playbooks (`playbooks/site.yml` for one-time bootstrap, `playbooks/update.yml` for manual `apt full-upgrade`), three local roles (`common`, `user`, `unattended_upgrades`), a single-device inventory targeting `reachy-mini.local`, and a Taskfile-driven developer toolchain.
- Conforms to the *single-environment-bootstrap* profile of `spec/ansible/playbook-development/` in `nolte/claude-shared`.

**Inside the boundary**:

- All Ansible source: `playbooks/`, `roles/`, `inventory/` (incl. `inventory/group_vars/`), `ansible.cfg`, `requirements.yml`, `requirements.txt`.
- Developer workflow surface: `task venv | galaxy | ping | bootstrap | update | lint | test | docs`, `--ask-pass`, `--ask-become-pass`, the `pass`-based lookup for the SSH public key.
- Repository hygiene: `.github/` workflows / settings / Probot configs (`settings.yml`, `release-drafter.yml`, `boring-cyborg.yml`, `stale.yml`), `mkdocs.yml` + `docs/`, `CLAUDE.md`, `README.md`, `LICENSE`, `.pre-commit-config.yaml`, `renovate.json5`.
- Spec-conformance with `nolte/claude-shared` (project-structure, playbook-development → *single-environment-bootstrap* profile).

**Explicitly outside**:

- The Reachy Mini hardware, its firmware, and the Pollen Robotics behavior runtime / `reachy_mini` Python SDK.
- WiFi / network onboarding of the device *before* SSH on `reachy-mini.local` is reachable (factory-side onboarding).
- Reachy Mini behavior development, dance choreography, Home Assistant bridge — those live in the `claude-reachy-mini` plugin scope.
- Cross-portfolio reusable workflows in `nolte/gh-plumbing` (referenced, not authored here).
- Generalisation to other Raspberry-Pi-class devices — this repo is intentionally *single-environment-bootstrap*, one device class.

## Audiences

Each entry: label, relationship category, interaction surface, expectation,
open questions, `confirmed` or `assumed`, criticality (primary / secondary /
peripheral). Mark a whole category as `none — <reason>` when it does not apply.

### Direct consumers

- **`nolte` (repo author, first operator on his own device)** — _category_: direct-consumer · _surface_: Taskfile workflow (`task bootstrap`, `task update`), direct repo edits · _expects_: reproducible runs against his own Reachy Mini; drift surfaces immediately · _status_: `confirmed` · _criticality_: primary
  - Open questions: none
- **Reachy Mini owner / hobbyist** — _category_: direct-consumer · _surface_: `task bootstrap` / `task update` CLI, README, `docs/index.md` · _expects_: one-command path from "fresh Reachy with SSH reachable" to "hardened, locale-aware, auto-patched device" without having to read Ansible code · _status_: `assumed` · _criticality_: secondary
  - Open questions: are non-`nolte` owners actually using this repo, or is it personal-only at this point? Is the repo intended for re-use, or is the public-repo shell only portfolio consistency?

### Operators

- **`nolte` as device-operator of his own Reachy Mini** — _category_: operator · _surface_: `task update`, `task ping`, manual `ansible-playbook` overrides, SSH for debugging · _expects_: idempotent re-runs; operator-controlled reboots (`unattended_upgrades_automatic_reboot: false`); no silent password disable; `pass`-based key resolution rather than committed keys — all per `CLAUDE.md` conventions · _status_: `confirmed` · _criticality_: primary
  - Open questions: is there a *second* operator (additional Reachy-Mini owner in the household, occasional helper)? If not, day-2 burden stays single-person.
- **Reachy Mini owner as day-2 operator** (updates, re-imaging) — _category_: operator · _surface_: same Taskfile flow, primarily `task update`, occasionally `task bootstrap` after hardware swap · _expects_: documented prerequisites (`pass`, SSH key, sudo password), clear failure messages, recovery-friendly defaults · _status_: `assumed` · _criticality_: secondary
  - Open questions: what's the day-2 path on hardware replacement? Inventory edit only, or full re-bootstrap?
- **GitHub Actions CI** as peripheral operator — _category_: operator · _surface_: `.github/workflows/ci.yml` plus release / docs delivery workflows · _expects_: green lint / syntax / dry-run before merge; reproducible docs build · _status_: `confirmed` · _criticality_: peripheral
  - Open questions: none

Deliberately omitted: the `unattended-upgrades` systemd daemon on the Pi — it's a *configured subject*, not a stakeholder with expectations of this repo.

### Contributors / maintainers

- **`nolte` as sole maintainer** — _category_: contributor · _surface_: local edits, commits on `develop`, PR review · _expects_: low-friction edit-loop (Taskfile, lint pre-commit), spec-conformant layout, `CLAUDE.md` as honest source of conventions (no aspirational claims that aren't backed by the referenced spec) · _status_: `confirmed` · _criticality_: primary
  - Open questions: none
- **Claude / AI agent as co-author** — _category_: contributor · _surface_: `CLAUDE.md`, `.claude/settings*.json`, skills from `nolte-shared` and `claude-reachy-mini` plugins · _expects_: clear spec references; `task lint` / syntax-check as ground-truth gate; reproducible sub-workflows · _status_: `confirmed` (Co-Authored-By trailers in commits; parallel sessions observable) · _criticality_: primary
  - Open questions: none
- **Renovate bot as dependency-update author** — _category_: contributor · _surface_: `renovate.json5` (extends `gh-plumbing` preset); generates PRs for `requirements.txt` and Galaxy roles in `requirements.yml` · _expects_: appropriate group / automerge rules; clean pin resolution; Mend dashboard as fallback visibility · _status_: `assumed` (config exists; whether the Renovate App is installed and actually opening PRs has not been verified) · _criticality_: peripheral
  - Open questions: is the Renovate GitHub App installed on this repository (verifiable via the App's repo selection or the Mend dashboard)?

Deliberately omitted: Boring-Cyborg / Release-Drafter / Stale apps — these are tools acting on behalf of the maintainer, not stakeholder authors in their own right.

### Governing parties

- **`nolte/claude-shared` spec portfolio** — _category_: governing-party · _surface_: `spec/project/project-structure/`, `spec/ansible/playbook-development/` (incl. *single-environment-bootstrap* profile), `spec/project/branching-model/`, `spec/project/pull-request-workflow/` · _expects_: conformance, or a documented exception; spec-driven reviews via `project-structure-apply` / `skill-review` · _status_: `confirmed` · _criticality_: primary
  - Open questions: none
- **Pollen Robotics as hardware vendor** — _category_: governing-party · _surface_: shipped image (Raspberry Pi OS Bookworm), factory `pollen` user, `reachy-mini.local` mDNS name, Reachy firmware update mechanisms · _expects_: bootstrap steps don't destroy vendor-critical config; SSH password auth stays enabled (no lockout) · _status_: `assumed` · _criticality_: secondary
  - Open questions: does Pollen Robotics publish an explicit list of Pi-side configurations that are essential to Reachy functionality and must not be overwritten? (If yes, that should land in `README.md` or a `meta/main.yml` comment in `roles/common`.)
- **Debian / Raspberry Pi Foundation as upstream** — _category_: governing-party · _surface_: APT origins, security advisories, codename lifecycle (Bookworm → Trixie), reboot requirement on kernel updates · _expects_: `unattended_upgrades_origins` formulated correctly; operator-controlled reboot policy (no silent reboot) · _status_: `assumed` · _criticality_: peripheral
  - Open questions: none
- **Legal / Privacy / Compliance / Business stakeholders** — `none — not applicable`. The repository is a private hobby bootstrap for a single device, with no personal-data processing and no commercial context. The methodology spec explicitly permits `none` with reason.

### Indirect audiences

- **Reachy Mini behavior developer** (de facto: `nolte` wearing the `claude-reachy-mini` plugin hat) — _category_: indirect · _surface_: works on the Pi that this repo bootstraps — Python, locale, SSH access, unattended-upgrades policy are all fixed here; feels reboots / locale drift / package conflicts directly inside `reachy_mini`-SDK sessions · _expects_: bootstrap installs enough tooling for `reachy_mini`-SDK sessions; no auto-reboots mid-demo (`unattended_upgrades_automatic_reboot: false` is explicitly there for this); `pollen` user keeps the group memberships the SDK needs · _status_: `confirmed` · _criticality_: primary
  - Open questions: none
- **End users of the Reachy Mini in everyday use** (people interacting with the Reachy via voice / vision / behaviors) — _category_: indirect · _surface_: none toward the repo — they only experience the effects · _expects_: device is available when they need it; predictable reboot windows; no surprising locale-driven misbehavior in speech output · _status_: `assumed` · _criticality_: secondary
  - Open questions: none
- **Downstream specs and skills** (`readme-structure`, future SLA / threat-model specs) — _category_: indirect · _surface_: read this `AUDIENCES.md` as input for their own artifact generation · _expects_: complete category coverage, clear `confirmed` / `assumed` tagging, repo-local position (not in a central registry) · _status_: `confirmed` (the audience-identification spec itself names these skills as consumers) · _criticality_: peripheral
  - Open questions: none

Deliberately omitted: other devices on the local network (Home Assistant, other Pis, IoT) — only background hygiene noise (mDNS hostname conflicts, traffic spikes during full-upgrade), no stakeholder with concrete expectations of this repo. Re-evaluate if a Home Assistant integration in the same LAN starts depending on Reachy availability windows.

## Open questions (cross-cutting)

- Is the repository intended for re-use by other Reachy Mini owners, or is the public-repo shell purely portfolio consistency? The answer changes whether the Reachy-Mini-owner audiences are realistic secondary stakeholders or aspirational ones.
- Does Pollen Robotics publish an explicit list of Pi-side configurations essential to Reachy functionality, so the bootstrap can document what it deliberately doesn't touch?
- Is the Renovate GitHub App actually installed on this repository, or does the configuration sit unused?
- Is there a second operator in scope (additional household member, occasional helper)?

## Revisit triggers

<!-- Events that should cause this list to be re-run via the `revisit` op:
     new public API, new deployment target, new regulated data class, new stakeholder, ... -->

- A second physical Reachy Mini enters scope (single-device → small fixed fleet within the same profile, but operator and indirect-audience picture shifts).
- Promotion out of the *single-environment-bootstrap* profile into *multi-environment-fleet* (e.g. a staging device is introduced before the production device).
- Extraction of any inline role under `roles/<name>/` into its own role repository (the moment a second consumer materializes — per the playbook-development spec, extraction is then MUST). The extracted role gets its own audience artifact.
- Pollen Robotics publishes a vendor-side bootstrap or hardening guide that overlaps with this repo's scope.
- Home Assistant or another co-tenant on the LAN starts depending on Reachy availability windows or shared credentials.
- The repository is published / promoted publicly (e.g. linked from a Reachy Mini community channel) — Reachy-Mini-owner audiences turn from `assumed` into something verifiable.
- A new `nolte/claude-shared` spec lands that materially constrains this repo (e.g. a future SLA or threat-model spec).
