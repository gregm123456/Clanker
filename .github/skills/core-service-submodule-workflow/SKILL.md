---
name: core-service-submodule-workflow
description: Create and wire a new Clanker-owned service repository as a submodule; use when starting a new Python service destined for a Raspberry Pi 5 or Zero 2W node.
argument-hint: Provide service name, node target (Pi5 / Zero2W / both), short purpose, and GitHub account/org.
---

# Core Service Submodule Workflow

Use this skill when creating a brand-new Clanker-owned service that will live in its own GitHub repository and be tracked here as a submodule.

This is distinct from the reference submodule workflow. Reference repos are read-only external inputs. Core service repos are Clanker-owned, writable, and maintained here.

## When To Use

- Starting a new Python service for a Raspberry Pi 5 or Zero 2W node.
- Creating a new standalone hardware-control, AI inference, sensing, or coordination service.
- Any new Clanker service that needs its own repository and deployment lifecycle.

## Decision Scope

Before proceeding, confirm the following with the user.

| Question | Why it matters |
|---|---|
| Service name | Becomes the GitHub repo name, local folder name, and submodule path |
| Node target: Pi 5, Zero 2W, or both | Determines scaffold weight, dependency policy, and startup constraints |
| Service purpose (1–2 sentences) | Drives README, systemd unit description, and operator docs |
| GitHub account or org | Used for `gh repo create` and the submodule URL |
| Does it need a systemd unit? | Required for services that must start at boot |
| Offline-capable? | Whether the service can run without Tailscale or internet |
| Update path | Default is `git pull` in the submodule; note any exceptions |

## Node Target Constraints

### Raspberry Pi 5

- Heavier Python dependencies are acceptable (numpy, torch, ONNX, etc.).
- Can run local AI inference workloads.
- VS Code Remote SSH over Tailscale is a valid development workflow.
- systemd services are typical.
- No special startup-time restrictions.

### Raspberry Pi Zero 2W

- Keep dependencies minimal. No large ML frameworks.
- Prefer pure-Python or very lean compiled dependencies.
- Fast startup is important — avoid slow import chains.
- Development is via shell over Tailscale SSH, not VS Code Remote.
- Systemd services must be lightweight; avoid long-running memory-heavy processes.
- If you are tempted to pull in a heavy framework on a Zero 2W, stop and ask whether the work belongs on a Pi 5 instead.

## Guardrails

- Place new core service submodules under `services/` to keep them visually distinct from `references/`.
- The service repo is Clanker-owned: commits, edits, and deployments happen in the submodule's own repo.
- Never mix heavy Pi 5 dependencies into a service that will run on a Zero 2W.
- Keep secrets, tokens, and device-specific credentials out of the repository. Direct them into `.env` files covered by `.gitignore`, or document a provisioning step.
- The systemd unit description and README must name the node target explicitly.

## Intake Checklist

1. Confirm service name (no spaces; use hyphens: `my-service-name`).
2. Confirm node target: Pi 5, Zero 2W, or both.
3. Confirm GitHub account or org for repo creation.
4. Confirm service purpose in one or two sentences.
5. Confirm whether a systemd unit is needed.
6. Confirm offline-capable or internet-dependent.
7. Confirm update path (default: `git pull` inside submodule).

## Command Templates

### 1. Create the GitHub repository

Requires the `gh` CLI authenticated with the target account.

```bash
# Create a new private repo (adjust --public/--private as needed)
gh repo create <github-account>/<service-name> \
  --private \
  --description "<one-line service purpose>" \
  --clone
```

Or without auto-clone if initializing locally:

```bash
gh repo create <github-account>/<service-name> \
  --private \
  --description "<one-line service purpose>"
```

### 2. Scaffold the service locally

```bash
mkdir -p <service-name>
cd <service-name>

# Minimal Python scaffold
git init
cat > README.md << 'EOF'
# <service-name>

**Node target:** Raspberry Pi <5 | Zero 2W>

<One or two sentences describing what this service does.>

## Setup

```bash
pip install -r requirements.txt
```

## Running

```bash
python main.py
```

## Update path

```bash
git pull
```

## Operator notes

- Node: Raspberry Pi <5 | Zero 2W>
- Connectivity: <Tailscale | local-only | internet-required>
- Offline-capable: <yes | no>
EOF

# .gitignore
cat > .gitignore << 'EOF'
__pycache__/
*.py[cod]
*.egg-info/
dist/
build/
.env
*.log
venv/
.venv/
EOF

# Lean requirements file (adjust for node target)
touch requirements.txt

# Entry point placeholder
# MIT License
cat > LICENSE << 'EOF'
MIT License

Copyright (c) 2026 Greg Merritt

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
EOF

# Entry point placeholder
cat > main.py << 'EOF'
# <service-name> — Raspberry Pi <5 | Zero 2W> service
# TODO: implement service logic
EOF

git add .
git commit -m "Initial scaffold for <service-name>"
```

### 3. Push to GitHub

```bash
git remote add origin https://github.com/<github-account>/<service-name>.git
git push -u origin main
```

### 4. Add as a submodule in Clanker root

Run from the Clanker root:

```bash
git submodule add https://github.com/<github-account>/<service-name>.git services/<service-name>
git submodule status
```

### 5. Verify and commit in Clanker root

```bash
git status --short
git add .gitmodules services/<service-name>
git commit -m "Add core service submodule: services/<service-name>"
```

### 6. Sync for other operators after pulling Clanker

```bash
git submodule sync --recursive
git submodule update --init --recursive
```

## Optional: systemd Unit Template

For services that start at boot. Place this file in the service repo at `deploy/<service-name>.service`.

```ini
[Unit]
Description=<service-name> — Clanker service (<node target>)
After=network.target

[Service]
Type=simple
User=pi
WorkingDirectory=/home/pi/<service-name>
ExecStart=/usr/bin/python3 /home/pi/<service-name>/main.py
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

Install on the device:

```bash
sudo cp deploy/<service-name>.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable <service-name>
sudo systemctl start <service-name>
```

## Documentation Template

Add this to `services/README.md` (or create it if it does not exist).

```md
### Service: <service-name>

- Repository: https://github.com/<github-account>/<service-name>
- Location: services/<service-name>
- Node target: Raspberry Pi <5 | Zero 2W>
- Purpose: <short description>
- Connectivity: <Tailscale | local | internet>
- Offline-capable: <yes | no>
- Update path: `git pull` inside `services/<service-name>` on the device
- systemd unit: <yes — deploy/<service-name>.service | no>
- Boundary: Clanker-owned service; commits go here, not in references/
```

## Expected Output

- GitHub repository created with correct visibility and description.
- Local scaffold committed and pushed.
- `LICENSE` (MIT, Greg Merritt) included in service repo.
- Submodule entry added to `.gitmodules` and committed in Clanker root.
- `services/README.md` entry created or updated.
- systemd unit file included in service repo if applicable.
- Any operator follow-up noted (device provisioning, secret setup, submodule init).
