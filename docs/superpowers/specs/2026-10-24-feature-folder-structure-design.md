# Feature folder structure design

## Intent

Organize the future Vue prototype by product area so session, settings and history code can stay together as each screen is implemented.

## Goals and constraints

- Keep `src/App.vue`, `src/main.ts` and `src/assets/` at their current paths.
- Replace the empty type-based placeholders with `src/features/session/`, `src/features/settings/`, `src/features/history/` and `src/shared/`.
- Add no screen code, UI, dependency or empty subfolder; create nested folders only when files need them.
- Keep Figma-first approval for any future visible UI implementation.

## Approaches

1. **Group by technical type** (`components/`, `views/`, `composables/`, `types/`, `fixtures/`). This matches Vue tutorials but spreads each feature across several folders.
2. **Group by product area — selected.** Use `features/session/`, `features/settings/`, `features/history/`, and `shared/` for genuinely cross-feature code. Keeps each area cohesive while the MVP grows.

## Builder decisions

Keep the two application entry files and shared CSS in place. Remove only the empty placeholders created for the previous scaffold layout. Place future screen-specific components, state, types and fixtures inside their feature; add subfolders only when files justify them.
