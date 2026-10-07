<p align="center">
  <img src="docs/images/logo.svg" width="72" height="72" alt="StepUpOnto SKSE mark">
</p>

<h1 align="center">StepUpOnto SKSE — patched</h1>

<p align="center"><strong>Same step-up. Hands off during SkyParkour.</strong></p>

<p align="center">
  Compatibility rebuild of StepUpOnto SKSE by TheShinyHaxorus.<br>
  Companion to <a href="https://github.com/SenjuWoo/Modern-NPC-Pathing">NPC Pathing NG</a>. Step-up behaviour is unchanged from original 1.5.
</p>

<p align="center">
  <a href="https://github.com/SenjuWoo/StepUpOntoSKSE-patched/actions/workflows/build.yml"><img src="https://github.com/SenjuWoo/StepUpOntoSKSE-patched/actions/workflows/build.yml/badge.svg" alt="Build"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-f0b060?labelColor=0d0f11" alt="MIT License"></a>
  <a href="https://github.com/SenjuWoo/StepUpOntoSKSE-patched/releases/tag/v1.5.1"><img src="https://img.shields.io/badge/release-v1.5.1-f0b060?labelColor=0d0f11" alt="v1.5.1"></a>
  <img src="https://img.shields.io/badge/CommonLibSSE--NG-b93280e-8f9aa6?labelColor=0d0f11" alt="CommonLibSSE-NG pin">
</p>

<p align="center">
  <a href="#install">Install</a>
  ·
  <a href="#build">Build</a>
  ·
  <a href="#honest-status">Honest status</a>
  ·
  <a href="https://github.com/SenjuWoo/Modern-NPC-Pathing">NPC Pathing NG</a>
  ·
  <a href="https://www.nexusmods.com/skyrimspecialedition/mods/175689">Original Nexus</a>
</p>

## Why it exists

While a SkyParkour animation is driving an actor (player parkour, or NPC parkour via [NPC Pathing NG](https://github.com/SenjuWoo/Modern-NPC-Pathing)), the character controller reads as **grounded and not moving**. That is exactly StepUpOnto's step trigger.

The original build could fire its step-warp mid-climb or mid-vault and fight the parkour animation's position alignment.

NPC SkyParkour, EVG marker routes, and navmesh unstuck failsafes live in **[Modern-NPC-Pathing](https://github.com/SenjuWoo/Modern-NPC-Pathing)**. This repo is only the StepUpOnto compatibility DLL.

## What this build changes

This build checks the `SkyParkourOngoing` animation graph variable in both the player and NPC step gates (`CanPlayerAttemptStep` / `CanNPCAttemptStep`) and stays hands-off until the parkour move finishes.

That is the only behaviour change.

One internal change with no user impact: the SimpleIni library dependency was replaced with a built-in INI reader/writer (same file, same keys, same format).

Without SkyParkour installed, this build behaves identically to the original.

## Install

Install over the original [StepUpOnto SKSE](https://www.nexusmods.com/skyrimspecialedition/mods/175689), letting the DLL overwrite. Existing `StepUpOntoSKSE.ini` and all settings keep working.

Requirements (same as the original):

- Skyrim SE/AE
- SKSE64
- Address Library for SKSE Plugins

Output path when you build locally: `package/SKSE/Plugins/StepUpOntoSKSE.dll`.

## Build

Windows x64, Visual Studio 2022 or 2026 C++ build tools, CMake 3.21+.

Requires a **built** CommonLibSSE-NG checkout at the same pin NPC Pathing NG uses: `b93280e832f263dbef44e44cbe2936622a02f91a`. See [Modern-NPC-Pathing/BUILDING.md](https://github.com/SenjuWoo/Modern-NPC-Pathing/blob/main/BUILDING.md).

```powershell
cmake -S . -B build -G "Visual Studio 17 2022" -A x64 -DCOMMONLIB_SSE_ROOT=<path to extern/CommonLibSSE>
cmake --build build --config Release
```

If `-DCOMMONLIB_SSE_ROOT` is omitted, CMake searches a few sibling layouts, including an NPC Pathing NG `extern/CommonLibSSE` folder. CI clones that pin into `extern/CommonLibSSE` and passes it explicitly.

The compiled DLL is written to `package/SKSE/Plugins/StepUpOntoSKSE.dll`. This tree does not ship a prebuilt DLL in git.

## Project map

```text
src/main.cpp              SKSE plugin entry
src/StepUpManager.cpp     step gates + SkyParkourOngoing guard
src/Hooks.cpp             engine hooks
src/Settings.cpp          INI reader/writer (same keys as original)
src/Raycast.cpp           step detection rays
src/Menu.cpp              in-game menu
StepUpOntoSKSE.ini        default settings
package/SKSE/Plugins/     staged DLL output
```

## Honest status

Verified in this tree:

- CMake project version **1.5.1**
- `GetGraphVariableBool("SkyParkourOngoing", ...)` in both player and NPC step gates
- GitHub Actions build against pinned CommonLibSSE-NG

Not claimed:

- Any change to original step-up heights, velocities, or detection besides the parkour guard
- That this repo contains NPC Pathing NG itself
- In-game screenshots

## Permissions

Released under the original mod page's permissions: modification and improvement releases are allowed with credit to the original creator, upload to other sites is allowed with credit, Donation Points are allowed, and assets may not be sold or converted to other games. This build is free and credits the original author.

The license file in this repository is [MIT](LICENSE).

## Credits

- **TheShinyHaxorus** — original StepUpOnto SKSE: all step-up design and implementation. This is their work with a two-function compatibility guard added.
- SkyParkour V3 by Waffuru — the `SkyParkourOngoing` graph variable this build checks.
- Compatibility patch by karlo — [Modern NPC Pathing](https://github.com/SenjuWoo/Modern-NPC-Pathing).

## License

[MIT](LICENSE)
