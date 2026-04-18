---
name: reference-submodule-workflow
description: Safely intake and maintain reference repositories as read-only submodules in Clanker; use when adding, syncing, auditing, or documenting references.
argument-hint: Provide repo URL, target path under references/, and intent (add, sync, audit, or document).
---

# Reference Submodule Workflow

Use this skill for repeatable reference repository operations in Clanker.

## When To Use

- Adding a new external reference repository.
- Syncing or initializing existing reference submodules.
- Auditing submodule state and documenting current intent.
- Preparing an operator-facing note about what to consult in a reference repo.

## Guardrails

- Treat reference submodules as read-only inputs unless the user explicitly overrides this policy.
- Never edit files inside a reference submodule working tree during routine intake, sync, or audit tasks.
- Keep reference repositories under `references/` unless the user explicitly requests a different location.
- Keep edits limited to Clanker-owned files such as `.gitmodules`, root docs, and Clanker scripts/docs.

## Intake Checklist

1. Confirm repository URL and why it is being imported.
2. Confirm target path, preferably under `references/`.
3. Confirm what subsystem/files are expected to be consulted.
4. Confirm read-only boundary for the imported reference code.

## Command Templates

Use these templates and substitute placeholders.

Add a new reference submodule:

```bash
git submodule add <repo_url> references/<name>
git submodule status
```

Add pinned to a branch (if explicitly required):

```bash
git submodule add -b <branch> <repo_url> references/<name>
git submodule status
```

Initialize/sync after clone:

```bash
git submodule sync --recursive
git submodule update --init --recursive
```

Update reference pointers intentionally:

```bash
git submodule update --remote --recursive
git status
```

Audit current state:

```bash
git submodule status --recursive
git config --file .gitmodules --get-regexp '^submodule\..*\.(path|url)$'
```

## Documentation Template

Use this snippet in a Clanker-owned doc or README section.

```md
### Reference: <name>

- Source: <repo_url>
- Location: references/<name>
- Why imported: <short reason>
- What to consult: <specific folders/files or subsystem>
- Boundary: read-only reference input; implement adaptations in Clanker-owned code, not inside this submodule.
```

## Expected Output

- Commands executed.
- Files changed in Clanker root.
- Submodule status after operation.
- Any follow-up for operators (for example, run submodule init/update after pulling).
