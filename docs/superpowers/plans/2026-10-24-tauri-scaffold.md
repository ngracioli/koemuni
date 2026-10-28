# Tauri Scaffold Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create and verify the root-level Tauri 2, Vue, TypeScript, Vite and Tailwind scaffold for the Koemuni prototype.

**Architecture:** Generate the maintained Vue + TypeScript Tauri template in a temporary directory. Review its files, then merge only scaffold files into the repository root and install Tailwind through its Vite plugin. Preserve the existing docs and assets; implement no Koemuni screens or behavior.

**Tech Stack:** Tauri 2, Vue 3 Composition API, TypeScript, Vite, Tailwind CSS, npm, Rust stable.

**Spec:** `docs/superpowers/specs/2026-10-24-tauri-scaffold-design.md`

## Global Constraints

- Keep this phase mocked; add no capture, permissions, transcription, AI integrations, authentication, billing, remote backends, persistence or excluded window/distribution features.
- Keep Rust within the generated Tauri bootstrap; do not add custom native behavior or capabilities.
- Preserve existing project documentation and assets; use English code identifiers and documentation.
- Do not change system configuration or commit/push.
- Do not implement Koemuni application screens in this scaffold delivery.

## Review Focus

- Existing repository files are not overwritten during generation or merge — inspect the temporary output and destination paths before copying.
- Scaffold capabilities remain at the template minimum — inspect generated capability files for extra access.
- Tailwind is wired through the current Vite integration and imported CSS entry — prove with a production build.
- Actual package scripts are recorded, not assumed — read `package.json` before updating `README.md`.
- Native runtime success is not inferred from a frontend build — launch the generated Tauri development command and report any unavailable window/runtime check.

---

### Task 1: Generate and inspect the official scaffold

**Files:**
- Create temporary project outside the repository, e.g. `/tmp/koemuni-tauri-scaffold/`.
- No repository files changed in this task.

**Interfaces:**
- Produces the official Tauri 2 Vue + TypeScript npm template, with its file list and scripts inspected before merge.

- [x] Run `npm create tauri-app@latest` in the temporary parent and select project name `koemuni`, identifier `com.koemuni.app`, TypeScript / JavaScript, npm, Vue, TypeScript.
- [x] Install template dependencies in the temporary project and inspect `package.json`, generated Tauri config, capabilities and file list; confirm no destination files in the repository would be overwritten.

### Task 2: Merge scaffold and configure Tailwind

**Files:**
- Create/merge the generated frontend and `src-tauri/` scaffold files at repository root, excluding generator-owned docs that conflict with existing files.
- Modify generated frontend entry CSS and Vite configuration for Tailwind's Vite plugin.
- Modify `README.md` to document actual available development/build scripts.

**Interfaces:**
- Consumes the inspected temporary template from Task 1.
- Produces a root-level Tauri/Vue app with Tailwind available to future screen work.

- [x] Merge only reviewed scaffold files; preserve repository `README.md`, `AGENTS.md`, `docs/` and `assets/`.
- [x] Install `tailwindcss` and `@tailwindcss/vite` with npm and wire `tailwindcss()` into the existing Vite config; import Tailwind in the generated CSS entry.
- [x] Read the resulting `package.json` scripts and add their exact names and commands to the existing README without replacing its project description or documentation links.
- [x] Inspect generated Tauri capabilities and dependencies; remove the greeting command, opener plugin and unused template assets.

### Task 3: Verify scaffold

**Files:**
- No additional files unless verification reveals a scaffold integration defect.

**Interfaces:**
- Consumes the root scaffold from Task 2.
- Produces recorded frontend build and native development-launch results.

- [x] Run the actual frontend build script from `package.json`; expect a successful Vite production build.
- [x] Run the actual Tauri development script from `package.json`; confirm the app process starts and close it cleanly. Screen capture was unavailable because the compositor does not support the capture protocol.
- [x] Recheck `git status`, the file tree, `README.md` links/scripts and capability/dependency scope; report all checks and any unverified runtime compatibility.

## Verification record

- `npm run build`: passed; includes `vue-tsc --noEmit` and Vite production build.
- `npm audit --audit-level=high --offline`: passed; zero vulnerabilities found in the cached advisory database.
- `cargo fmt --all -- --check`, `cargo check --offline` and `cargo clippy --offline -- -D warnings`: passed.
- `npm run tauri dev`: Vite started, Rust compiled, and the Tauri process remained running until clean shutdown. Visual confirmation was unavailable: `grim` reported that the compositor does not support the screen-capture protocol.
- Application runtime compatibility beyond this development launch, including the documented Arch/KDE/Wayland/PipeWire target, remains unverified.
