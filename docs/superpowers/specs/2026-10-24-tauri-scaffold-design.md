# Tauri scaffold design

## Intent

Create a runnable project foundation for the Koemuni mocked desktop MVP, so later deliveries can add the approved Vue screens and behavior on top of the defined stack.

## Goals and constraints

- Generate the official Tauri 2 + Vue + TypeScript + Vite project, use npm, and add Tailwind through its Vite integration.
- Place project files at the repository root without replacing existing documentation or assets.
- Keep this delivery to generated bootstrap/configuration and necessary Tailwind setup. Add no MVP screens, interactions, services, integrations, persistence, or custom Rust behavior.
- Preserve the generator's minimum Tauri capabilities. Record actual package scripts in README.
- Run available frontend checks and launch the native development app when supported; report checks that cannot run.

## Non-goals and anti-goals

No application feature UI, session logic, fixture data, dependency beyond the selected scaffold and Tailwind requirements, packaging, system configuration changes, or compatibility claims based only on version checks.

## Approaches

1. **Generate in a temporary directory, then merge scaffold files into the repository root — selected.** Uses the official template while protecting the existing docs and assets; requires reviewing the generated file list and merging conflicts deliberately.
2. Generate directly in the non-empty repository. Fewer merge steps, but risks overwriting or refusing existing project files.
3. Hand-create the scaffold. Avoids merge work, but duplicates official template configuration and can drift from supported Tauri defaults.

The user selected approach 1. The environment check found stable Rust/Cargo, Node.js/npm, and GTK/WebKit development libraries available. This confirms tool presence only, not native runtime compatibility.

## Builder decisions

Use the official generator's Vue + TypeScript template with npm and Tauri 2, then add Tailwind using its current Vite integration. The selected template included a greeting screen, Rust command and opener plugin; remove those examples and leave the Vue root visually empty until a Koemuni screen is approved in Figma. Preserve README and existing docs, and document the actual resulting scripts.
