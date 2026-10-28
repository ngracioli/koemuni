# Implementation backlog

This backlog is the build sequence for the mocked MVP in [requirements](mvp.md). Complete one delivery at a time, keep each change reviewable and stop when its exit criteria pass. A delivery does not authorize work outside the MVP. Delivery 1's scaffold and checks are complete; Deliveries 2–7 remain pending.

## Rules for every delivery

- Read [AGENTS.md](../AGENTS.md), [requirements](mvp.md), [interface](interface.md), [architecture](architecture.md) and [design system](design-system.md) before implementation. These documents define scope, behavior, naming, state ownership and quality limits.
- Inspect `git status` and the files you plan to touch first. Preserve existing user changes. Keep the diff limited to the current delivery.
- Keep content fictional and state in memory. Every screen shows **“Modo demonstração — nenhum áudio está sendo capturado”**.
- Do not add capture, permissions, transcription, AI, network calls, persistence, authentication, billing, external integrations, custom Rust behavior, excluded window features, packaging or distribution.
- Use Vue 3 Composition API, `<script setup lang="ts">`, TypeScript, Vite, Tailwind CSS and the Tauri 2 bootstrap. Keep Rust limited to generated native startup.
- Before any interface change, follow the Figma-first process in [AGENTS.md](../AGENTS.md): confirm the intended screen, inspect the component library and complete the design in Figma before coding. Do not treat a backlog item as approval to skip that gate.
- Reuse the selected scaffold's scripts and dependencies. Do not add a library, abstraction or test framework unless a delivery requires it and existing tools cannot meet the acceptance criteria.
- For code changes, run the applicable checks listed in [architecture quality gates](architecture.md#quality-gates). For documentation-only changes, check links, consistency and whitespace. Report actual results and unverified environment limits.

## Delivery 0 — Prepare the development environment

**Purpose:** Confirm that the machine can build a Tauri 2 desktop app before generating files.

**Actions:**

1. Confirm the target is the documented initial environment: Arch Linux, KDE Plasma, Wayland and PipeWire. Treat compatibility as unknown until the app is run there.
2. Check whether Rust stable with `rustc`/`cargo`, Node.js LTS with `node`/`npm`, and Tauri's Linux development libraries are already available. Record installed versions; do not reinstall working tools.
3. If Linux dependencies are missing, use the current Arch package list in the [official Tauri prerequisites](https://v2.tauri.app/start/prerequisites/). The documented package command is:

   ```sh
   sudo pacman -S --needed webkit2gtk-4.1 base-devel curl wget file openssl appmenu-gtk-module libappindicator-gtk3 librsvg xdotool
   ```

4. If Rust or Node.js is missing, install Rust through the official [Rust install instructions](https://www.rust-lang.org/tools/install) and Node.js LTS through the distribution's supported method. Avoid shell commands that download and execute a script in one step.
5. Confirm the shell can find the tools:

   ```sh
   rustc --version
   cargo --version
   node --version
   npm --version
   ```

**Constraints:** These are instructions for the developer operating the machine; the agent must not install packages, alter system configuration, change shell startup files or claim the machine is compatible. Mobile tooling is not needed.

**Exit criteria:** Required commands are available, missing prerequisites and any install failures are recorded, and no environment compatibility claim is made from version checks alone.

## Delivery 1 — Generate and prove the scaffold

**Purpose:** Establish the smallest runnable desktop app using the agreed stack.

**Current status (2026-10-24):** Implemented. The root scaffold and scripts exist. Frontend build, Rust formatting/check/Clippy and npm audit pass. The Tauri development process started and remained active until clean shutdown; direct visual inspection was unavailable because the compositor does not support the screen-capture protocol. Arch/KDE/Wayland/PipeWire runtime compatibility beyond this launch remains unverified.

**Actions:**

1. Recheck repository status and ensure the scaffold will not overwrite existing user files. The repository currently contains project documentation, so inspect the generator's behavior for a non-empty destination before choosing how to run it.
2. Use the official [`create-tauri-app`](https://v2.tauri.app/start/create-project/) flow. Select Vue, TypeScript, npm, and Tauri 2. Do not create a second nested project or replace repository documentation.
3. Add Tailwind CSS using its current Vite integration and official setup instructions. Keep the dependency set to what the selected template and required stack need.
4. Inspect the generated file tree, capabilities, scripts and dependency versions. Keep native capabilities at the scaffold minimum; do not add plugins or application commands.
5. Record the actual script names in `README.md`; do not guess command names before inspecting `package.json`.
6. Run the scaffold's frontend development/build checks and launch the native app with its generated Tauri development script. Confirm the window opens and close it cleanly.

**Exit criteria:** A Vue/TypeScript/Vite/Tailwind frontend and Tauri 2 bootstrap exist at the repository root; the native development window opens; the actual run and build commands are documented; no mock-MVP feature has been implemented yet.

## Delivery 2 — Add the application shell and navigation

**Purpose:** Make the three MVP screens reachable before adding session behavior.

**Scope:** App shell, in-memory screen selection, Session / Settings / History placeholder views, Portuguese navigation labels, shared demonstration notice and responsive scrolling for the proposed desktop dimensions. Use the approved Figma screens and existing library components.

**Do not add:** Router, Pinia, new windows, persistence, session controls or fictional data beyond what is needed to show screen structure.

**Exit criteria:** All three views open by keyboard; focus is visible; each view displays the exact demonstration notice; navigation does not reset root-owned state (even if only placeholder state exists); applicable typecheck and build pass.

## Delivery 3 — Implement the session lifecycle and transcript

**Purpose:** Complete FR-02 and FR-03 from [requirements](mvp.md).

**Scope:** One root-owned reactive session state, fixture transcript, start/pause/resume/end transitions, active-time duration, ordered delayed transcript lines, fixture exhaustion, timer cleanup and invalidation on new session/disposal. Follow [interface timing rules](interface.md#timers-and-pending-work).

**Before editing:** Locate every caller of session actions and existing timer owner. Keep the transition and timer rules in one composable/state owner; do not put scheduler logic in screen components.

**Focused proof:** Verify start creates a fresh ID and empty transcript; pause freezes duration and arrivals; resume schedules exactly one continuation; ending active or paused sessions prevents later mutation; exhausted fixtures stop adding lines while duration continues; disposal clears timers. Use a small deterministic test/assertion check for this logic if current tooling permits.

**Exit criteria:** FR-02/03 pass, transitions and timer ownership are documented in code where not self-evident, focused checks and applicable typecheck/build pass.

## Delivery 4 — Implement simulated replies

**Purpose:** Complete FR-04 and FR-05 without calling a model, API or network service.

**Scope:** Predefined suggestion, nonblank custom-question submission, loading/success/error states, duplicate-request prevention, deterministic fixture error followed by retry, preservation of prior content, and cancellation on pause/end/new session/disposal. Keep question and response together in session data.

**Do not add:** Random failures, provider interfaces, reusable async frameworks, settings for reply behavior or external calls.

**Focused proof:** Verify blank input is blocked, both actions cannot overlap while loading, simulated failure is repeatable and retry succeeds, and a canceled/queued callback cannot append a late reply. Keep cancellation and request-state ownership shared for the two request types.

**Exit criteria:** FR-04/05 and relevant interface states pass; earlier transcript and replies survive errors; focused race/cancellation checks and applicable typecheck/build pass.

## Delivery 5 — Implement settings

**Purpose:** Complete FR-06 using fictional values only.

**Scope:** Fictional microphone/output choices and editable assistant instructions, owned in app memory and preserved when switching screens. Display that values last only for the current execution.

**Do not add:** Device enumeration, microphone APIs, permission prompts, save buttons/services, storage, or behavior changes to the predefined replies.

**Exit criteria:** Values remain after navigation, reset to fixtures after reload, controls have labels and work by keyboard, and applicable typecheck/build pass.

## Delivery 6 — Implement history and archive snapshots

**Purpose:** Complete FR-07 and FR-08.

**Scope:** Initial fictional history fixtures, list/details navigation, empty list and no-suggestions states, and one immutable snapshot per ended session. A new session must not mutate prior entries.

**Focused proof:** Verify initial fixtures; details; empty states; ending before the first transcript line; ending paused/active sessions; no duplicate entry on repeated end; completed replies included; new sessions independent; reload restores fixture history and removes newly ended sessions.

**Exit criteria:** FR-07/08 pass with details read-only and archive exactly-once behavior; relevant state checks and applicable typecheck/build pass.

## Delivery 7 — MVP walkthrough and closeout

**Purpose:** Prove the implemented scope against the requirements and remove inconsistencies in docs.

**Actions:**

1. Complete the [manual review script](#manual-review-script) on the actual application.
2. Run available frontend format/lint/typecheck/build checks, focused state/timer checks and applicable native checks. Do not install overlapping tooling solely to satisfy a metric.
3. Inspect capabilities, dependencies and source for accidental capture, permissions, network calls, persistence or excluded functionality.
4. Update README and relevant docs with actual scripts, behavior, checks and limitations. Explicitly say if Arch/KDE/Wayland/PipeWire runtime compatibility was not exercised.

**Exit criteria:** All FR-01–FR-08 acceptance criteria have evidence; available gates pass or have a specific recorded limitation; no unsupported compatibility claim or out-of-scope feature remains.

## Manual review script

1. Open each screen by keyboard. Check visible focus, labels and the exact demonstration notice. Confirm the initial transcript/reply states and fictional history.
2. Start a session. Observe ordered lines from “Você” and “Outros” and advancing duration. Navigate away and back; confirm continuity without duplicate arrivals.
3. Pause and wait longer than a fixture delay. Confirm duration and transcript stay unchanged. Resume and confirm one continuation. Let the fixture finish; duration should continue while active.
4. Request a suggestion and submit a custom question. Confirm loading and predefined answers. Try blank input, duplicate submission, deterministic error and retry.
5. Pause or end while a reply is loading. Wait beyond its delay; confirm no late answer. Start another session and confirm old callbacks cannot affect it.
6. End active and paused sessions, including one before its first line. Confirm one history entry per session with final duration, transcript and completed replies. Open details; start another session and confirm old details stay unchanged.
7. Change settings and navigate between screens. Reload: settings and newly added history reset to fixtures. Check empty-history and no-suggestion states.
8. Review the application and dependency/capability configuration for real capture, permissions, network integrations, persistence or excluded features.

## Implementation notes

- Follow the Figma-first project rule for every UI delivery. The backlog's design scope does not itself approve a design or implementation.
- No specific script names, tool availability or successful checks are assumed before scaffold. Record what the generated project actually provides.
- The target environment is an initial proposal. A frontend build does not prove native runtime support on Arch Linux, KDE Plasma, Wayland or PipeWire.
- Keep tasks small. If a delivery reveals a requirement conflict or missing product decision, update the relevant requirements/interface documentation before coding beyond that delivery.
