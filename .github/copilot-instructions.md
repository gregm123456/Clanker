# Clanker Copilot Instructions

Clanker is the home base repository for Raspberry Pi coding projects used in the Clanker art installation.

## Project Context

- Treat this repository as coordination and workflow context for a fleet of Clanker nodes, not as a single deployable app.
- Default to Python for Raspberry Pi software unless the task clearly calls for another language or target platform.
- Assume the primary runtime targets are Raspberry Pi 5 and Raspberry Pi Zero 2W.
- ESP32 devices may also be part of the system; keep their constraints and toolchains separate from Raspberry Pi assumptions.
- Clanker is an experiential generative AI installation. Prefer designs that keep AI inference local on Raspberry Pi 5 hardware unless the user explicitly asks for a cloud dependency.

## Node Roles

- Raspberry Pi 5 nodes are the main compute nodes. They are expected to run heavier Python services, local AI workloads, and NPU-adjacent logic.
- Raspberry Pi Zero 2W nodes are for lightweight input, output, sensing, control, and coordination tasks.
- Do not assume software written for a Pi 5 is appropriate for a Zero 2W. Call out CPU, memory, storage, startup time, and dependency weight when relevant.
- When proposing architecture, separate node responsibilities clearly and prefer moving heavy work off the Zero 2W.

## Development Workflow

- Assume this repository will largely manage or reference git submodules for individual Pi software components and services.
- Place sample, reference, vendor, or research repos in clearly named submodule areas that distinguish them from Clanker-owned runtime code.
- Prefer an explicit folder structure such as `references/`, or another clearly documented location rather than mixing reference repos into primary Clanker service directories.
- Prefer changes that preserve a clean separation between submodules, node-specific services, shared tooling, and deployment documentation.
- When adding automation or scripts, make node targeting explicit so it is clear which code runs on Pi 5, Zero 2W, ESP32, or the development Mac.
- Favor operationally simple workflows that work well for a small fleet of physical devices.

## Reference Repositories

- Treat imported sample or reference repositories as source material, examples, hardware-driver references, model integration references, or prior-art implementations.
- Treat reference submodules as read-only inputs for Clanker work unless the user explicitly states otherwise.
- Do not plan or propose routine edits, cleanup, version bumps, formatting passes, dependency changes, or direct code changes inside reference submodules.
- Do not frame work as contributing back to reference repositories. Clanker may learn from them, but it does not maintain them.
- Do not treat reference repositories as automatically authoritative for Clanker architecture, coding style, deployment approach, security posture, or hardware assumptions.
- Adapt code intentionally. Prefer extracting patterns, interfaces, protocol details, hardware usage, and setup knowledge over copying large chunks wholesale.
- Implement Clanker-owned adaptations in Clanker codebases or Clanker submodules rather than patching the reference repo itself.
- When code is derived from a reference repo, keep the Clanker-specific adaptation explicit in surrounding docs or commit context.
- Keep reference repos isolated as submodules whenever practical so they can be updated, inspected, or removed independently.
- If a reference repo is large, avoid loading broad swaths of it into agent context. Search narrowly, read only the relevant files, and summarize findings before making changes.
- When discussing a large submodule, identify the exact directories or files that matter instead of reasoning over the whole repo.
- Prefer lightweight notes, local documentation, or extracted design summaries in the main repo when recurring reference knowledge is needed from a very large submodule.
- If a large reference repo risks overwhelming the working context, explicitly narrow the task to a subsystem, device driver, model wrapper, or service boundary before proceeding.

## Connectivity And Access

- Tailscale is the default connectivity layer for Clanker devices.
- For Raspberry Pi 5 nodes, assume VS Code Remote SSH over Tailscale is a valid workflow.
- For Raspberry Pi Zero 2W nodes, do not assume VS Code Remote SSH is reliable; prefer lightweight shell-based workflows.
- For Zero 2W access, assume the development Mac can shell in with `<username>@<tailscale_host>` using Tailscale SSH.
- When suggesting remote workflows for Zero 2W nodes, optimize for minimal resource usage and simple terminal commands.

## Deployment And Updates

- Assume Zero 2W nodes typically update code by pulling from GitHub in the relevant submodule rather than by full remote IDE workflows.
- Prefer deployment suggestions that fit a `git pull` style update path for lightweight nodes unless the user asks for something more elaborate.
- Keep secrets, credentials, tokens, and device-specific sensitive configuration out of GitHub.
- When secrets or machine-local settings are required, direct them into ignored env files, secure provisioning steps, or documented manual setup.

## Coding Guidance

- Keep dependencies lean, especially for code that may run on Zero 2W hardware.
- Prefer straightforward, debuggable Python over framework-heavy solutions unless a framework is clearly justified.
- Be explicit about OS services, hardware interfaces, and Linux assumptions when writing setup instructions.
- If a task touches multiple node types, document which behavior belongs to each node.
- When relevant, note operational constraints such as offline operation, intermittent connectivity, boot-time reliability, and recoverability after power loss.

## Documentation Expectations

- Treat hardware assumptions, node role, deployment target, and update path as first-class documentation concerns.
- When creating scripts, services, or docs, include enough context that a future operator can tell where the code belongs in the Clanker fleet.