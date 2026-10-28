# Koemuni

Koemuni is a planned desktop meeting assistant with transcription and reply suggestions. **The repository now contains an empty desktop scaffold; the prototype screens and behavior have not been implemented yet.** The first phase is a mocked interface prototype.

The prototype will validate navigation, readability and meeting controls using fictional data and Vue state held only in memory. It will not validate transcription accuracy or AI response quality. Reloading or closing the application will discard changes.

Every screen must clearly display: **“Modo demonstração — nenhum áudio está sendo capturado”**.

## Defined stack

- Tauri 2, with Rust limited to the native Tauri bootstrap.
- Vue 3 Composition API and `<script setup>`, TypeScript and Vite.
- Tailwind CSS.
- Initial target environment: Arch Linux, KDE Plasma, Wayland and PipeWire. Compatibility has not been tested.

## Current scope

Three screens: Session, Settings and History. Simulate session controls, progressive transcript lines, predefined suggestions and custom-question replies. Keep settings and completed sessions in memory only.

No real audio or screen capture, microphone permissions, transcription, AI APIs or models, authentication, billing, remote backend, Axum, PostgreSQL, SQLite, other persistence or external integrations. Global shortcuts, hiding from screen sharing, always-on-top windows, deployment, updates and installer distribution are also excluded.

## Documentation

Start here:

- [Project instructions](AGENTS.md)
- [Implementation backlog](docs/backlog.md)

Product and implementation references:

- [Design system and visual identity](docs/design-system.md)
- [Prototype requirements and acceptance criteria](docs/mvp.md)
- [Interface and interaction rules](docs/interface.md)
- [Proposed architecture](docs/architecture.md)

The development prerequisites and scaffold sequence are documented in the [backlog](docs/backlog.md).

## Development commands

- `npm run dev` starts the frontend in a browser.
- `npm run build` type-checks the frontend and creates a production build.
- `npm run tauri dev` starts the desktop app in development mode.
- `npm run preview` serves the production build locally.
