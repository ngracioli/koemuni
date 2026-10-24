---
name: figma-visual-quality
description: Use when creating or editing Figma screens, reusable components, brand lockups, or visual assets where alignment and consistency must hold across frames.
---

# Figma visual quality

Build frames from the existing design system and verify their visible geometry before treating them as finished. Load `figma-use` before each `use_figma` call.

## Before composing

- Inspect the target frames, their parents, and the component library. Find reusable components and the actual source assets first.
- Use library instances for repeated controls and brand patterns. Extend an existing component when it owns the same responsibility; add a small reusable component when the pattern repeats and no suitable component exists.
- Treat a component as a match only when it has the same responsibility and can express the required asset, spacing and alignment; don't bend an unrelated component into the pattern.
- Read visible bounds as well as node bounds. Raster assets can have transparent padding, so equal frame centers do not guarantee equal-looking marks.
- When reusing an image fill, inspect its visible alpha bounds, `scaleMode`, `scalingFactor` and `imageTransform`; copied fills can retain a scale that makes artwork much smaller than its frame. For lockups, crop or re-import assets to their visible artwork bounds, size each frame to that cropped aspect ratio, then verify the rendered result.
- Keep the target frame's existing hierarchy, spacing, typography and layout conventions. Avoid one-off offsets when Auto Layout or a shared alignment anchor expresses the relationship.

## Aligning a brand lockup

- Place the symbol and wordmark in one reusable horizontal lockup. Use a consistent gap and align their optical centers; account for transparent image padding and the wordmark's text bounds.
- Judge the whole lockup beside the target header at presentation size. Scale symbol and wordmark to carry similar visual weight to nearby header labels and controls without crowding them; tune spacing from visible artwork bounds, not image-frame edges. As a starting check, keep the visible gap at or below roughly one-quarter of the symbol's visible width, then review the balance in context.
- Provide size variants only when actual frames need different lockup sizes. Keep symbol, gap and wordmark relationships consistent across variants.
- Use Koemuni's supplied `koemuni-symbol.png` wherever the symbol appears. Never recreate it as a typed “K”. Use `koemuni-app-icon.png` only where a square app icon is intended.
- Put the brand lockup in the reusable-components page and replace repeated hand-built marks with component instances.

## Verify each result

- Inspect metadata for component type, dimensions, parent, spacing, and alignment constraints. Check the visible artwork bounds when transparent padding affects alignment.
- Render a screenshot of each changed frame after composing. Inspect it at presentation size for vertical centering, baseline balance, consistent gaps, clipping, unexpected scaling, and missing or substituted brand marks.
- Compare repeated instances across every affected screen and the component specimen. Fix the shared component when the same defect appears in multiple instances.
- Return created and mutated node IDs from every `use_figma` write. Re-read metadata and screenshots after each meaningful edit; do not count a successful script call as visual validation.
- Report the frames checked and any visual state or asset that could not be verified.
