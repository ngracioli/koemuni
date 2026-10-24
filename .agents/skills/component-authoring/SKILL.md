---
name: component-authoring
description: Use when creating or extracting Vue UI components in Koemuni, including reusable controls, component APIs, props, variants, accessibility, Tailwind styling, or component folder placement.
---

# Koemuni component authoring

Create Vue components that have a clear responsibility, a small typed API, and a real reuse case. Follow the project's Figma-first workflow for every visible UI change. This skill defines the code organization and implementation after the intended design is known.

## Before creating a component

1. Read the project instructions and relevant requirements, interface, architecture, and design docs. Inspect existing components and every caller that may use the new API.
2. Identify the Figma component and the states/variants it represents. If the Figma library lacks a needed reusable component, state this before creating it.
3. **Before creating a component in Figma or code, tell the user its proposed name, why reuse justifies it, where it will live, and the initial props/events.** This is a heads-up; proceed after the notice unless the user redirects the work. Do not silently create a component or ask for approval a second time when the target design was already confirmed.
4. Use the smallest API that covers approved designs and actual callers. Add a prop only when it changes a meaningful visual or behavioral state. `disabled`, color/tone, and size are useful when the design or callers need those states; they are not mandatory boilerplate on every component.

## Folder placement

Use the narrowest existing location that matches ownership. The layout below is provisional until the project architecture and Vue scaffold exist:

```text
src/
  components/
    ui/                 # Generic visual primitives used across features: Button, Badge
    shared/             # Koemuni patterns shared by multiple screens: DemoNotice
    session/            # Session-only components: SessionControls, TranscriptList
    settings/           # Settings-only components
    history/            # History-only components
  views/                # Screen composition and screen-level orchestration
```

Create folders only when a real component needs them. Put a component in `ui/` only when it is independent of Koemuni feature data and has a stable reuse case. Put app-specific components in `shared/` only when multiple features/screens use them; keep components used by one feature in that feature's folder. A view/page composes components and owns screen-level layout; it should not become a dumping ground for reusable controls. Do not add barrel files, generic `common/`, `utils/`, or empty folders.

If a component is used once and has no distinct responsibility, keep its markup in the caller. Do not move existing components solely to match this provisional example.

## Update this guide after scaffolding

The folder tree above is a starting proposal, not a permanent project rule. As soon as the project architecture is finalized and the Vue scaffold exists, update this skill and the architecture documentation to match the actual source tree, naming, and component conventions. Inspect the generated scaffold and implemented components first; replace provisional paths and examples rather than layering a second competing structure on top.

After that update, place each new component beside its owning feature/page by default. Use the project's shared location only when more than one feature/page needs the same responsibility. Use the UI primitives location for feature-independent controls reused across the app. When a component starts to serve multiple features, move it to the shared location and update its imports/callers in the same change. Keep each page's feature components in its designated feature folder; do not place feature-specific UI in a global components bucket.

## Component API

- Use Vue 3 Composition API, `<script setup lang="ts">`, typed `defineProps` and `defineEmits`. Props flow down; events flow up. Do not mutate props or hide business state inside a presentational component.
- Keep required content in the default slot when callers need to supply arbitrary content; use named slots only for distinct, repeated composition points. Prefer native elements and attributes when they already provide the behavior.
- Use finite union types for variants such as `size: 'sm' | 'md' | 'lg'` and `tone: 'primary' | 'secondary' | 'danger'`. Set a sensible default only when it matches the design. Avoid boolean combinations that permit conflicting states.
- Prefer semantic names (`tone`, `size`, `loading`, `disabled`) over styling implementation details (`purple`, `largePadding`). Keep props focused: no prop for a one-off caller need unless the variation is part of the component's responsibility.
- Preserve useful native attributes and listeners. Bind `$attrs` to the interactive root where appropriate; do not swallow `aria-*`, `id`, `name`, `type`, or event listeners. For a button, set `type="button"` unless submit behavior is explicitly requested, and use the native `disabled` attribute.
- Keep layout and business rules with their owner. Components render state and emit intent; the parent/composable owns session transitions, fixture data, timers, and requests.

Example for a reusable button when Figma and callers require these variants:

```vue
<script setup lang="ts">
withDefaults(defineProps<{
  size?: 'sm' | 'md'
  tone?: 'primary' | 'secondary'
  disabled?: boolean
}>(), {
  size: 'md',
  tone: 'primary',
})

defineEmits<{ click: [event: MouseEvent] }>()
</script>

<template>
  <button
    type="button"
    :disabled="disabled"
    :class="[
      'rounded-md font-medium focus-visible:outline focus-visible:outline-2',
      size === 'sm' ? 'px-2 py-1 text-sm' : 'px-3 py-2',
      tone === 'primary' ? 'bg-purple-600 text-white' : 'bg-neutral-800 text-white',
      disabled && 'cursor-not-allowed opacity-50',
    ]"
    @click="$emit('click', $event)"
  >
    <slot />
  </button>
</template>
```

Adapt the example to the confirmed design tokens, interaction, and actual callers. Do not copy variants that are not needed.

## Tailwind, accessibility, and efficiency

- Keep Tailwind class names statically discoverable. Map finite variants to complete class strings; do not construct classes such as `` `bg-${color}-600` `` dynamically.
- Reuse project tokens and existing class patterns. Extract repeated markup only when it represents the same responsibility; do not build a generic styling system for one control.
- Keep components shallow and templates declarative. Derive display values with `computed` only when useful; do not add watchers for values that can be derived directly.
- Use semantic HTML, keyboard-operable controls, visible focus, labels, and adequate accessible names. A color change must not be the only way to convey state. Apply the native `disabled` state when interaction is unavailable; use `aria-disabled` only when a control must remain focusable and implement its disabled behavior explicitly.
- Do not add dependencies, render functions, dynamic component registries, or memoization without a measured need. Vue already optimizes common component use; profile before adding performance machinery.

## Completion check

Before reporting completion, inspect all callers and verify the component matches the confirmed Figma variants. For code changes, run the available typecheck, build, and task-appropriate checks required by the project instructions. For a documentation-only skill edit, check Markdown links, consistency, and whitespace; do not claim application checks ran.

## References

- [Vue component props](https://vuejs.org/guide/components/props.html)
- [Vue component events](https://vuejs.org/guide/components/events.html)
- [Vue performance](https://vuejs.org/guide/best-practices/performance.html)
- [Tailwind adding custom styles](https://tailwindcss.com/docs/adding-custom-styles)
