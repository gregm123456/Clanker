# Clanker Services

This directory contains Clanker-owned service repositories tracked as git submodules. Each service has its own GitHub repository, deployment lifecycle, and node target.

For reference repositories (read-only external inputs), see `references/`.

---

### Service: Clanker_clipboard

- Repository: https://github.com/gregm123456/Clanker_clipboard
- Location: `services/Clanker_clipboard`
- Node target: Raspberry Pi Zero 2W
- Purpose: Clipboard input device — reads four 8-position rotary knobs (MCP3008 ADC/resistor ladder), multiple push buttons, and continuously updates a 6-inch grayscale IT8951 ePaper display. Serves live input state over the local network to other Clanker system components.
- Connectivity: Tailscale (local network); ePaper and input work offline
- Offline-capable: yes (input and display work; state serving requires local network)
- Update path: `git pull` inside `services/Clanker_clipboard` on the device
- systemd unit: yes — `deploy/clanker-clipboard.service`
- Boundary: Clanker-owned service; commits go here, not in `references/`
