# Clanker

Home-base repository for the Clanker art installation software ecosystem.

This repo is a coordination root for Raspberry Pi node projects and related submodules.

## Scope

- Clanker node architecture and operational conventions
- Submodule organization for Clanker-owned services and external references
- Shared project-level documentation and workflow guidance

## Status

Starter repository initialized.

## Copilot Custom Agents

Workspace-level custom agents are defined in `.github/agents/`.

**Reference workflows (read-only):**
- `reference-scan`: read-only scanning of external/reference repos and submodules.
- `reference-submodule-intake`: adds or updates reference repos as submodules (prefer `references/`) without editing reference repo code.

**Core service workflows (Clanker-owned):**
- `core-service-init`: creates a new Clanker-owned service repository on GitHub, scaffolds it for the target node (Pi 5 or Zero 2W), and wires it as a submodule under `services/`.

## Copilot Skills

Workspace-level skills are defined in `.github/skills/`.

- `reference-submodule-workflow`: reusable playbook for adding, syncing, auditing, and documenting reference submodules while preserving read-only boundaries for reference code.
- `core-service-submodule-workflow`: reusable playbook for creating a new Clanker-owned service repo, scaffolding it for the correct Pi node target, pushing to GitHub, and wiring it as a submodule under `services/`.

## Submodule Organization

- `services/` — Clanker-owned runtime services. Each subdirectory is a submodule pointing to a GitHub repo that Clanker maintains. Code here is writable and deployed to Pi nodes.
- `references/` — External reference repositories, treated as read-only inputs. Scope docs live in `references/README.md`.

## Reference Submodules

Reference-repository scope docs live in `references/README.md`, including import intent, what to consult first, and the read-only boundary for all reference code.