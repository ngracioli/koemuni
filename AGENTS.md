# Project instructions

- All project documentation must be written in English. Preserve intentional Portuguese product UI examples and original external asset/Figma names and links.
- Write code identifiers in English.
- Use English filenames, including documentation files.
- Before preparing or executing any commit, you MUST read and follow the [commit skill](.agents/skills/commit/SKILL.md).
- Before any application UI change, follow the [Figma-first UI skill](.agents/skills/figma-ui-workflow/SKILL.md): locate and confirm the intended Figma screen with the user, audit the reusable component library, and finish the design in Figma before implementing it.

## Koemuni prototype

- Before implementing, read [README.md](README.md), [requirements](docs/mvp.md), [interface](docs/interface.md), [design system](docs/design-system.md), [architecture](docs/architecture.md) and [backlog](docs/backlog.md).
- Keep the current phase mocked: fictional data and Vue state in memory only. Show “Modo demonstração — nenhum áudio está sendo capturado” on every screen.
- Use the defined stack: Tauri 2 native Rust bootstrap, Vue 3 Composition API with `<script setup>`, TypeScript, Vite and Tailwind CSS. Keep Rust within bootstrap for this phase.
- Do not introduce real capture, microphone permissions, transcription, AI integrations, authentication, billing, remote backends, persistence, external integrations or excluded window/distribution features. Follow the exclusions in the requirements.
- Add no services, dependencies or functionality unless necessary for the authorized task. Folder structure and visual direction are initial proposals, not reasons for extra abstractions or a global state manager.
- Do not change system configuration. Treat Arch Linux, KDE Plasma, Wayland and PipeWire as the initial target, not verified compatibility.
- Inspect existing files and working-tree changes first. Preserve user edits; touch only files required by the task.
- Update documentation when behavior changes. Keep documentation in English and product UI text in Brazilian Portuguese.
- Run available checks appropriate to the change. For implementation, prioritize typecheck, build and the manual script; test state/timer logic where justified. For documentation-only edits, check links and consistency without installing tooling.
- Report actual verification results and unavailable checks. Never invent commands, passing tests or environment validation.
- Do not commit or push unless explicitly requested.
