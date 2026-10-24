---
name: figma-ui-workflow
description: Require every Koemuni UI change to be represented in Figma first; confirm the intended screen, audit the component library, reuse or add components, then implement against the approved design.
---

# Figma-first UI workflow

Use this skill for every user-visible application UI change: screens, components, layout, styling, copy, visual states and interaction states. This applies to new UI and edits to existing UI, including small fixes. The Figma file is the source of truth for what gets implemented.

## Required sequence

1. **Read project rules and inspect current work.** Follow `AGENTS.md`. For Koemuni implementation, read `README.md`, `docs/mvp.md`, `docs/interface.md`, `docs/architecture.md` and `docs/backlog.md`. Inspect the current Figma pages, relevant frame and existing code before editing.
2. **Locate the design and confirm the target.** Find the likely screen/frame in the project's Figma file and inspect its visual context. Ask the user: “Encontrei `[frame]` em `[página]`. É essa tela que você quer usar/alterar ou está se referindo a outra?” Wait for their answer before changing the frame or implementing UI. If no matching frame exists, explain that and ask which existing screen or new frame they intend. Do not guess and start coding.
3. **Audit the Figma component library.** Before composing or changing the screen, inspect the library for every reusable control and repeated pattern the screen needs. Record which components and variants match. Reuse matching instances. If a needed reusable component is missing, announce its proposed name, reuse reason, and variants/properties to the user before creating it; then create it in the library with the smallest useful API and compose the screen from that component. This is a heads-up, not an extra approval gate: proceed unless the user redirects the work. If a component nearly matches, extend it with a variant or property rather than creating a competing duplicate. Keep visual tokens bound to shared variables where available. Follow the `component-authoring` skill for code organization and API design.
   For composition and alignment checks, also follow [figma-visual-quality](../figma-visual-quality/SKILL.md).
4. **Finish the screen in Figma first.** Represent all visible structure and every implemented state in Figma, including relevant empty, loading, error, disabled, focus and selected states. Use library instances for reusable controls and patterns. A unique content block does not need to become a reusable component unless it repeats or the existing system defines it as one. Check the composition visually, not from metadata alone. Provide the user a link to the confirmed frame when practical.
5. **Implement against that frame.** Inspect existing application components and callers. Every reusable control or pattern used from the Figma library must be implemented as a corresponding Vue component or mapped to an existing Vue component with the same responsibility; do not replace it with repeated inline markup. Follow the `component-authoring` skill for folder placement, typed props/events, and variants. Reuse existing code components where they represent the same responsibility. Do not change product behavior outside the request or project scope.
6. **Compare and close the loop.** After implementation, compare the rendered UI with the Figma frame, correct material differences in Figma first and then code, and run the project checks required by `AGENTS.md`. Report the Figma frame, library components reused/added, actual checks, and anything unverified.

## Stop conditions

- If the intended Figma frame is not confirmed, wait before making UI changes.
- If Figma access is unavailable or no design can be established, do not implement the UI outside Figma. Explain the blocker; unrelated non-UI work may continue if independently authorized.
- If the library audit finds no matching component, announce and add the reusable component to the Figma library before using it in the screen. Never silently skip the audit or leave a screen component as an undocumented one-off.
- Preserve Koemuni's mocked-only scope and all other project exclusions. A Figma design does not authorize implementation of excluded behavior.

## Figma tool prerequisites

- Load `figma-use` before every `use_figma` call.
- For composed screens or multi-section views, also load `figma-generate-design` and include both skill names in the call.
- Use the project's existing Figma file and page naming unless the user directs otherwise. Do not create a second design file just to avoid inspecting the current one.
