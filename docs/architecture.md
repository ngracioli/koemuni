# Proposed architecture

This is an initial folder and ownership proposal, not an existing scaffold. Keep the first implementation small and follow the actual Tauri/Vite scaffold once created. Naming and quality rules below apply to future code; they do not authorize scaffolding, tooling installation or implementation now.

## Naming rules

- Use English filenames, folder names and code identifiers, following [project instructions](../AGENTS.md). Product UI text remains Brazilian Portuguese.
- Vue components and views use PascalCase: `SessionControls.vue`, `SessionView.vue`. Name components by their responsibility, not vague labels such as `Common` or `Manager`.
- Composable filenames and exported functions use matching `use`-prefixed camelCase: `useSession.ts`, `useSession`. Other TypeScript modules use kebab-case: `session-types.ts`, `session-fixtures.ts`.
- TypeScript types/interfaces use PascalCase; variables and functions use camelCase; module-level constants representing fixed values use UPPER_SNAKE_CASE. Use descriptive names and explicit units such as `elapsedMs`.
- Rust modules/files, functions and variables use snake_case; types and traits use PascalCase; constants use UPPER_SNAKE_CASE. Module names must describe existing responsibilities.
- Documentation filenames use kebab-case, such as `architecture.md`. Preserve conventional tool filenames such as `README.md`, `AGENTS.md`, `Cargo.toml` and `tauri.conf.json`, and original external asset names.
- Use `.test.ts` for TypeScript tests when supported by the chosen tooling. Rust unit tests stay with their module; add `src-tauri/tests/` only for justified integration tests.

## Responsibilities

- **Vue components:** render the three screens, expose labeled controls and emit user actions. Do not embed fictional conversation data in templates.
- **Composables:** own reactive state, session transitions, elapsed time, delayed transcript lines and simulated reply requests.
- **Types:** describe the small domain shared by components, fixtures and composables.
- **Fixtures:** contain fictional devices, initial settings, transcript scenarios, historical sessions and predefined replies, including a deterministic reply-error scenario.
- **Rust:** remains in the Tauri 2 bootstrap. No custom audio commands, permissions, backend or domain logic in this phase.

## State ownership

Create one reactive prototype-state owner at the application root, using Vue primitives and composables. Pass state and actions to screens through props/events as needed. Do not add Pinia or another global state manager initially.

Keep the active session, settings and history alive across screen changes. Only the root-owned session simulation schedules session timers; follow the lifecycle rules in [Interface](interface.md). At application disposal, cancel all timers and pending callbacks.

Create mutable runtime copies of fixtures. Archive a snapshot when ending a session so new sessions cannot mutate history. Reloading recreates settings and initial history from fixtures. Do not use localStorage, sessionStorage, IndexedDB, files or databases.

## Small folder proposal

```text
src/
  App.vue
  main.ts                 # Vue entry point
  components/             # SessionControls.vue, TranscriptList.vue
  views/                  # SessionView.vue, SettingsView.vue, HistoryView.vue
  composables/            # usePrototypeState.ts, useSession.ts when needed
  types/                  # session-types.ts
  fixtures/               # session-fixtures.ts, reply-fixtures.ts
  assets/                 # Shared styles and local assets, when needed
src-tauri/
  src/
    main.rs               # Generated native executable entry point
    lib.rs                # Generated Tauri application bootstrap
  capabilities/           # Scaffold capabilities; minimum required access
  icons/                  # Scaffold assets only; no distribution work
  build.rs                # Generated Tauri build integration
  Cargo.toml              # Native manifest
  tauri.conf.json         # Tauri configuration
docs/                     # Requirements, interface, architecture and backlog
```

Paths illustrate intended responsibilities; create only files needed by the actual implementation. Keep generated configuration at its standard location. Do not create empty directories, barrel exports, generic `utils` collections or speculative native layers such as repositories, services and commands.

Use simple in-memory screen selection initially; no routing dependency is required for three screens. Extract components or split composables only when existing responsibilities justify it. Keep frontend behavior in `src/`; do not mirror it in Rust. Native modules, if a later authorized phase needs them, live under `src-tauri/src/` and follow Rust module conventions rather than copying Vue folders. The native layout follows the [Tauri project structure](https://v2.tauri.app/start/project-structure/).

## Conceptual types

Identifiers below are proposed English code names; visible labels remain Brazilian Portuguese.

| Type | Essential data |
| --- | --- |
| `Session` | Unique ID, start/end timestamps when applicable, status (`inactive`, `active`, `paused`, `ended`), active duration, transcript lines and completed suggestions. |
| `TranscriptLine` | ID, speaker (`self` or `others`, displayed as “Você” or “Outros”), text and active-time offset. |
| `Suggestion` | ID, kind (`reply` or `customQuestion`), question when applicable, predefined answer and active-time offset. |
| `Settings` | Fictional microphone ID, fictional audio-output ID and assistant instructions. |

Keep request state (`idle`, `loading`, `success`, `error`), draft question and timer handles separate from archived domain data. Settings only affect the demonstration form; device choices and assistant instructions invoke no integration and do not generate new reply content.

No local HTTP server as application architecture, remote infrastructure, persistence layer, provider interfaces or speculative abstractions. The scaffold's development tooling is distinct from an application backend.

## Shared code quality

- Give each function/component one clear responsibility. Prefer direct control flow, explicit state transitions and guard clauses over nested conditionals, boolean flags and clever one-liners.
- Cyclomatic complexity must be **at most 10 per handwritten function**, in TypeScript and Rust. It measures independent control-flow paths, not just the number of `if` statements. Keep conditional/loop nesting to **at most three levels**; count the outermost control block as level one. Prefer shallower code even below these limits.
- When a limit is exceeded, simplify conditions or extract a cohesive operation with a meaningful name. Do not split code mechanically just to satisfy a metric, suppress the rule or replace readable conditions with obscure expressions. Use exhaustive `switch`/`match` for state-dependent behavior where clearer.
- Keep one owner for each business rule, state transition, fixture source and timer lifecycle. Before adding logic, search existing implementations and callers. Reuse an existing helper/component when it represents the same responsibility.
- Consolidate duplicated behavior that must change together, including suggestion and custom-question request handling. Similar-looking markup alone does not justify a generic component; avoid abstractions for hypothetical reuse.
- Remove unused imports, variables and helpers introduced by the change. Do not mix unrelated cleanup into a feature or bug fix. Comments explain non-obvious reasons, not restate code.
- Optimize demonstrated work: avoid repeated transformations during rendering, redundant state and unnecessary allocations. Measure an actual bottleneck before adding caches, concurrency or complex data structures.
- Validate external inputs at their boundary when any are introduced in an authorized phase. Never render arbitrary HTML, swallow errors or weaken permissions to make a check pass.

## Vue and TypeScript practices

- Use Vue 3 Composition API with `<script setup lang="ts">`. Enable strict TypeScript checking in the scaffold; type props, emits and domain boundaries. Avoid `any`, unchecked type assertions and non-null assertions that hide invalid states; narrow `unknown` where needed.
- Treat props as read-only and emit actions to the state owner. Avoid mutating nested props or exposing mutable global module state. Model mutually exclusive statuses with unions rather than contradictory booleans.
- Keep templates declarative. Move repeated derived expressions into pure `computed` values; use watchers for necessary side effects, not to synchronize values that can be derived. This follows [Vue computed-property guidance](https://vuejs.org/guide/essentials/computed.html).
- Preserve reactivity when accessing state; do not destructure reactive objects in ways that disconnect updates. Keep `computed` getters free of mutation, timers and asynchronous requests.
- Use stable domain IDs as list keys. Avoid array-index keys for changing lists, broad deep watchers and functions that repeatedly transform an entire transcript in templates.
- Register lifecycle cleanup for effects owned by a component/composable. Follow the session timer and callback invalidation rules in [Interface](interface.md); mounting another screen must not create another session scheduler.
- Use Tailwind utilities consistently. Extract repeated visual structures into meaningful components when justified, rather than copying large utility blocks or building an unused design system.
- Preserve semantic controls, labels, focus and keyboard access. Loading/error feedback must not remove content or unexpectedly move focus.

## Rust and Tauri practices

- Keep the generated bootstrap small. Do not add native business logic, HTTP servers, shared state, plugins or commands for behavior already handled by the mocked Vue interface.
- Prefer safe, idiomatic Rust: explicit ownership, borrowed references when ownership is unnecessary, enums for exclusive states and exhaustive `match` expressions. Avoid unnecessary `clone`, allocations, shared mutability and lifetime machinery.
- Use `Result` and `?` for recoverable failures; preserve useful error context instead of ignoring results. Avoid `unwrap`, `expect` and `panic!` in handwritten recoverable paths. An intentional fatal bootstrap failure must explain why startup cannot continue; generated bootstrap behavior need not be rewritten without cause. See [Rust error handling](https://doc.rust-lang.org/book/ch09-00-error-handling.html).
- Do not introduce `unsafe` code, background threads or an asynchronous runtime for the prototype. A future need requires a separately justified change, not anticipatory infrastructure.
- Keep Tauri capabilities minimal. Do not enable shell, filesystem, microphone, network or other extra access for mocked controls. Preserve scaffold security settings; never disable them to bypass an error.

## Quality gates

A change is ready only when applicable checks pass and its acceptance criteria are demonstrated. These are project review gates, not claims that automation already exists. During authorized scaffolding, document the actual scripts and available tooling; use existing tools first and do not install overlapping tools solely for a metric.

| Gate | Passing condition |
| --- | --- |
| Scope and architecture | Diff stays within the authorized task; naming, state ownership, fixture separation and mocked-only limits hold. No unrelated edits or unnecessary dependency additions. |
| Readability and duplication | Changed functions meet complexity ≤10 and nesting ≤3; related behavior has one owner. Review callers and repeated logic, not just changed lines. No suppressions added just to bypass checks. |
| Frontend static checks | Available formatter check, Vue/TypeScript lint and strict typecheck pass. When configuring a complexity rule, verify it covers both `.ts` and Vue script blocks. No new warnings or unchecked types in changed code. |
| Native static checks | For Rust changes, rustfmt check, Cargo check and Clippy pass for applicable targets. Resolve warnings introduced by the change; preserve generated bootstrap and avoid unrelated cleanup. Clippy metrics do not replace the separate cyclomatic-complexity review. |
| Behavioral proof | Relevant manual acceptance criteria pass. State and timer logic has focused deterministic tests or executable assertions for transitions, pause/resume, cleanup, late callbacks and exactly-once archiving. No arbitrary coverage percentage or static-markup test quota. |
| Build | Available frontend build passes for implementation changes. Native changes and final prototype review also require the applicable native build check. Exclude installer packaging and distribution. |
| Accessibility and usability | Affected screens retain keyboard navigation, labels, visible focus, readable states and the demonstration notice. Use the [manual script](backlog.md#manual-review-script). |
| Documentation and report | Behavior changes update relevant docs. Report actual checks and results, unavailable tooling, baseline failures and environment limitations. Never imply a frontend build proves native compatibility. |

Until a metric tool is available, manually review independent paths and nesting and record that limitation; do not report an automatically measured score. Automated complexity rules should eventually encode these project limits where supported, while Rust's complexity review remains explicit if no matching analyzer is available.

For documentation-only changes, verify local links, consistency and whitespace; application checks are not required. A failed or unavailable required implementation check leaves its gate unverified: report the reason and do not claim completion of that gate. Do not weaken rules, fabricate results or change system configuration to obtain a pass.
