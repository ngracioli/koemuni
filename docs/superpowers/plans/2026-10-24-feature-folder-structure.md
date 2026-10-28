# Feature Folder Structure Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace generic empty placeholders with feature-oriented roots for session, settings, history and shared code.

**Architecture:** Keep `src/App.vue`, `src/main.ts` and `src/assets/` at their current paths. Create one root per product area under `src/features/` plus `src/shared/`; add no nested folders until code needs them. Update the architecture proposal to match.

**Tech Stack:** Vue 3, TypeScript, Vite, Tailwind CSS.

**Spec:** `docs/superpowers/specs/2026-10-24-feature-folder-structure-design.md`

## Global Constraints

- Keep Rust within the generated Tauri bootstrap.
- Do not implement UI, screens, application logic or dependencies in this folder-only delivery.
- Preserve existing application entry paths and shared CSS path.
- Keep future visible UI subject to the repository's Figma-first workflow.

## Review Focus

- `src/App.vue` and `src/main.ts` remain in place and their imports continue resolving.
- `src/assets/main.css` remains in place and Tailwind remains configured.
- Only placeholders from the previous empty scaffold structure are removed.
- New feature roots correspond only to the documented Session, Settings and History screens.
- `src/shared/` remains for genuinely cross-feature code; no unused nested abstractions are added.

---

### Task 1: Switch empty roots to feature organization

**Files:**
- Delete: `src/components/ui/.gitkeep`
- Delete: `src/components/shared/.gitkeep`
- Delete: `src/components/session/.gitkeep`
- Delete: `src/views/.gitkeep`
- Delete: `src/composables/.gitkeep`
- Delete: `src/types/.gitkeep`
- Delete: `src/fixtures/.gitkeep`
- Create: `src/features/session/.gitkeep`
- Create: `src/features/settings/.gitkeep`
- Create: `src/features/history/.gitkeep`
- Create: `src/shared/.gitkeep`
- Modify: `docs/architecture.md` folder proposal and ownership notes

**Interfaces:**
- Consumes the current empty scaffold folders and the approved design.
- Produces feature roots for future files; no application import paths change.

- [x] Remove only the empty `.gitkeep` placeholders listed above.
- [x] Create the four new feature/shared roots with `.gitkeep` placeholders.
- [x] Update the architecture proposal to group screen-specific components, composables, types and fixtures under each feature; reserve `shared/` for code used by multiple features.
- [x] Run `git diff --check` and `npm run build`; both passed.
- [x] Inspect `git status` and the tree to confirm entry paths, Tailwind files and Tauri files remain unchanged.
