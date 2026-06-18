# FiveM Optimizer Companion

FiveM Optimizer Companion is a Windows desktop application that helps players monitor and optimize their FiveM experience without touching game memory, injecting DLLs, or bypassing anti-cheat. It focuses on system-level tuning, cache management, and non-intrusive overlays to keep FiveM smooth and stable.

> Status: WIP – this README documents the planned feature set before implementation.

---

## Key Principles

- **Client-side only:** No DLL injection, no memory editing, no mod menus, no native call spam.
- **Anti-cheat friendly:** Uses standard Windows APIs, ETW, and file operations only; designed to behave like other legitimate optimizers and telemetry tools.
- **Reversible tweaks:** All changes are either temporary for the current session or come with clear reset paths.
- **Game-agnostic telemetry:** FPS, frametime, and system metrics are collected independently of FiveM, so the app can be useful for other games as well.

---

## Feature Overview

### 1. Performance Dashboard

A central view that shows your current system and game performance at a glance:

- Live FPS and frametime updates collected via ETW-style frame presentation tracking (similar to PresentMon).
- CPU, GPU, RAM, and disk usage for the system, with FiveM.exe highlighted.
- Network panel: ping to a configurable host (e.g., your favorite FiveM server), simple packet loss and jitter indicators.
- Active profile indicator (Low End / Balanced / High End / Custom).

The goal is to give you the same level of visibility you expect from tools like FPS overlays or generic PC optimizers, but tuned for FiveM workflows.

---

### 2. Non-Intrusive FPS & HUD Overlay

An RTSS-style overlay that never injects into the FiveM process:

- Drawn as a standard topmost transparent window or Xbox Game Bar widget, not as a game hook.
- Displays:
  - FPS (current / 1% low)
  - Frametime graph
  - Ping
  - CPU and GPU utilization percentages
- Fully configurable:
  - Corner selection (top-left/right, bottom-left/right)
  - Scale and opacity
  - Color theme that matches dark or light backgrounds

By relying on the OS and Game Bar widget APIs instead of hooking the render pipeline, the overlay stays compatible with anti-cheat and behaves similarly to official performance widgets.

---

### 3. Safe FiveM Cache & Cleanup Tools

FiveM’s cache and temporary files frequently cause stutters, texture streaming bugs, and long load times as they accumulate over time.
FiveM Optimizer Companion offers:

- One-click **"Clear FiveM Cache"**:
  - Locates `%localappdata%\FiveM\FiveM.app\data`.
  - Deletes only the known safe folders such as `cache`, `server-cache`, `server-cache-priv` (and optionally `nui-storage`) that community guides recommend clearing.
- Optional removal of log/crash files to reclaim disk space, inspired by existing cleaning tools like ClearFiveM and other FiveM cache cleaners.
- Clear explanation of what will be deleted and how FiveM will rebuild those files on next launch.

This module never touches game assets or scripts; it only operates on cache and temporary data that are safe to regenerate.

---

### 4. Windows Gaming Tweaks (Session-Focused)

A set of conservative, reversible tweaks focused on the Windows side rather than the game client:

- Power plan tuning:
  - Switch the system to a high-performance power scheme before play and optionally restore the previous plan afterward.
- Process priority:
  - Elevate FiveM.exe to a higher priority class to reduce CPU contention, without modifying memory or injecting code.
- Background noise reduction:
  - Identify CPU-heavy background processes and optionally lower their priority or suggest closing them, similar to general game optimizers.
- Optional toggles for non-critical Windows features that are commonly disabled in optimization guides (certain animations, indexing, telemetry), with a preset that applies only safe, documented changes.

Tweaks are designed to be understandable and undoable, avoiding any registry or service changes that could break normal system behavior.

---

### 5. FiveM Graphics & Config Presets

For players who prefer not to manually tweak every graphics slider, the app can manage a set of presets:

- Presets:
  - Low End: prioritize FPS and stability on weaker hardware.
  - Balanced: compromise between visuals and performance.
  - High End: keep higher quality while avoiding the worst bottlenecks.
- Presets operate by editing FiveM/GTA configuration files and launch options, not by patching memory or injecting natives.
- Each preset describes:
  - View distance and LOD bias
  - Shadow quality and distance
  - Reflections and post-processing
  - Anti-aliasing and motion blur recommendations
- "Restore default" button to revert to original settings.

This approach follows the pattern of existing FPS booster scripts and config guides, but moves the logic into a standalone companion app instead of server scripts.

---

### 6. Profiles System

A lightweight profile engine that bundles all optimizations into named configurations:

- Each profile stores:
  - Windows tweaks (power plan choice, background process preferences)
  - FiveM config preset
  - Overlay layout and style
  - Optional per-game overrides (for using the tool with other titles)
- Default profiles:
  - Low End: maximum stability and FPS.
  - Balanced: recommended for most systems.
  - High End: better visuals on strong hardware.
- Custom profiles:
  - Users can clone and edit profiles, then export/import them as JSON.

This design mirrors how general game optimizers provide "one-click modes" while still allowing advanced users full control.

---

### 7. Session Logs and Comparisons

To help you see whether changes actually help, the app can optionally log sessions:

- For each gameplay session:
  - Hardware snapshot (CPU, GPU, RAM)
  - Active profile and tweaks
  - Aggregated metrics: average FPS, minimum FPS, frametime variance, average CPU/GPU usage
- Simple comparison view:
  - "Before vs After" charts based on two selected sessions.
  - Answer questions like "Did the Balanced preset actually reduce stutter compared to default?"

All logs are stored locally and can be exported if you want to share results or file an issue.

---

## Non-Goals

To stay compatible with anti-cheat and server rules, FiveM Optimizer Companion will not:

- Inject DLLs into FiveM or GTA processes.
- Read or write game memory.
- Provide ESP, aimbot, or any gameplay-altering features.
- Manipulate FiveM resources or server-side scripts; server optimization belongs on the server, not the client.

---

## Planned Tech Stack (Draft)

The stack is still open to iteration, but the current plan is:

- **Core service:** Native binary (Rust or C++) for telemetry and Windows tweaks using official APIs and ETW.
- **Desktop UI:** Web-based (HTML/CSS/TypeScript) running inside a modern shell (e.g., Tauri or WebView2), inspired by existing gaming optimizers and dashboards.
- **Overlay:**
  - Option A: topmost transparent window drawn by the core process.
  - Option B: Xbox Game Bar widget using the Game Bar SDK, consuming telemetry from the core service.

Implementation details will be refined as the project evolves.

---

## Inspiration and Related Work

This project takes inspiration from:

- FiveM cache cleaners and optimizer utilities such as ClearFiveM and other dedicated cleaners.
- FPS booster scripts and client-side optimizers that adjust graphics settings for performance.
- Generic Windows gaming optimizers and game loop tuners that manage power plans, process priority, and background services.
- Telemetry tools like PresentMon for frame analysis without injection.

FiveM Optimizer Companion aims to bring these ideas together into a focused, anti-cheat-friendly tool tailored for FiveM players.