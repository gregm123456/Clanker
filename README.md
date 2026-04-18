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

Workspace-level custom agents are defined in `.github/agents/` for reference repository workflows:

- `reference-scan`: read-only scanning of external/reference repos and submodules.
- `reference-submodule-intake`: adds or updates reference repos as submodules (prefer `references/`) without editing reference repo code.

These agents are intended to enforce least privilege and keep reference repositories as read-only inputs.

## Copilot Skills

Workspace-level skills are defined in `.github/skills/`.

- `reference-submodule-workflow`: reusable playbook for adding, syncing, auditing, and documenting reference submodules while preserving read-only boundaries for reference code.

## Reference Submodules

Reference-repository scope docs live in `references/README.md`, including import intent, what to consult first, and the read-only boundary for all reference code.