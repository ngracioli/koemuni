# Koemuni Design System

Visual rules for the Koemuni desktop prototype and its Figma file. The interface is a fictional, in-memory demonstration: every screen must show **“Modo demonstração — nenhum áudio está sendo capturado”**.

## Intent

Make Koemuni feel like a calm, focused macOS utility: compact, restrained, familiar and easy to scan during a meeting. Use macOS as visual inspiration, not as a claim of native behavior. Keep the near-black desktop direction, purple as the primary accent and the prototype's three screens: Session, Settings and History. The Session window stays short and horizontal by default; secondary areas open in larger windows.

## Goals

- Keep the active conversation and primary session controls easy to find.
- Make transcript, replies and status readable at a glance without visual noise.
- Use consistent color, type, spacing and component states across all screens.
- Keep the interface accessible by keyboard and understandable without color alone.

## Non-goals

- Do not introduce screens, features, integrations or persistent data beyond the current MVP.
- Do not imitate Apple logos, product marks or proprietary assets.
- Do not imply that audio is captured, devices are connected, or AI is running.
- Keep interface surfaces and controls flat; reserve the logo's supplied purple gradient for brand artwork. Avoid glass effects, heavy shadows and animation without a clear usability purpose.

## Approaches considered

1. **Near-black editorial utility — selected.** Near-black system-like surfaces, violet action color, large italic conversation excerpts, outlined answer cards, system typography and familiar controls. Preserves the compact MVP direction and suits a focused meeting companion.
2. **Native-inspired light utility.** Similar hierarchy with light surfaces. Closer to many default macOS appearances, but conflicts with the project's documented dark direction.
3. **Hybrid dark dashboard.** Dark base with stronger panels and a more prominent sidebar. Offers more visual character, but risks feeling like a web dashboard instead of a compact desktop utility.

## Visual principles

- **Quiet hierarchy:** one clear primary action per state; secondary actions remain visually quieter.
- **Content first:** conversation excerpts and suggested answers receive more visual weight than chrome or decoration. Use a small uppercase label, an italic quote for the latest utterance and one restrained outlined card for the suggested answer.
- **Consistent alignment:** use the same 8 px spacing rhythm, 16 px panel inset and Inter preview styles throughout. Center button labels horizontally and vertically; keep reading content left aligned.
- **Compact, not cramped:** use short control groups and consistent spacing; keep text and hit targets comfortable.
- **Native-inspired, not platform-dependent:** use familiar desktop conventions without depending on macOS-only fonts or controls.
- **Honest states:** demo content, simulated activity and unavailable actions must be labeled clearly.

## Color tokens

Use these as the initial Figma variables. Keep tokens semantic so palette adjustments do not require component-by-component changes.

| Token | Value | Use |
| --- | --- | --- |
| `surface-canvas` | `#0A0A0B` | Main window background |
| `surface-sidebar` | `#111114` | Navigation area and secondary regions |
| `surface-panel` | `#101014` | Cards and grouped controls |
| `surface-raised` | `#1A1A20` | Hovered or elevated controls |
| `surface-input` | `#111114` | Text inputs and selects |
| `border-subtle` | `#2E2736` | Dividers and quiet outlines |
| `text-primary` | `#F5F2F8` | Main text and headings |
| `text-secondary` | `#B8B4C2` | Supporting text and timestamps |
| `text-muted` | `#77717F` | Low-priority labels; do not use for essential information |
| `accent-primary` | `#9333EA` | Primary action, selected navigation and links |
| `accent-hover` | `#7E22CE` | Hover or pressed state for primary accent |
| `accent-text` | `#C084FC` | Purple labels and inline emphasis on dark surfaces |
| `status-success` | `#30D158` | Successful or active state, paired with text/icon |
| `status-warning` | `#FFD60A` | Caution state, paired with text/icon |
| `status-error` | `#FF453A` | Error and destructive emphasis, paired with text/icon |

Use purple for primary actions and restrained emphasis. Do not use status colors as decoration or as the only indicator of meaning. Keep active status green and pair it with a text label. The demonstration notice uses a neutral info treatment with a purple marker.

## Typography

- Use a system UI sans-serif stack in the app: `-apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif`. Do not require San Francisco or an external font download. Figma specimens use Inter for reliable cross-platform preview; treat it as a visual stand-in for the app's system font stack.
- Use sentence case for labels and headings. Keep product copy in Brazilian Portuguese.
- Use tabular numerals for elapsed time and other values that update in place.
- Avoid long blocks of all-caps text, condensed faces and decorative display fonts.

| Style | Size / line height | Weight | Use |
| --- | --- | --- | --- |
| `title` | 22 / 28 px | 600 | Screen title |
| `section-title` | 16 / 22 px | 600 | Main content sections |
| `body` | 14 / 20 px | 400 | Answers and form values |
| `conversation-quote` | 17 / 24 px | 400 italic | Latest conversation excerpt |
| `label` | 13 / 18 px | 500 | Buttons, navigation and form labels |
| `caption` | 12 / 16 px | 400 | Supporting descriptions and timestamps |

Do not reduce essential interface text below 12 px. Let long transcript content wrap naturally; avoid truncating conversation text.

## Layout and spacing

- **Compact Session window:** use a short horizontal frame, currently **920 × 390 px** in Figma. Keep brand, status, elapsed time, session controls, latest transcript quote, outlined reply suggestion, custom question and the exact demo notice visible without a persistent sidebar.
- **Expanded secondary windows:** Settings and History use a larger desktop frame, currently **1200 × 800 px** in Figma. These areas may use denser forms, lists and scrollable content; they do not determine the default Session window size.
- Use a compact header to reach History and Settings. Keep the Session window focused on the current conversation and its immediate actions.
- This borrows the compact, separate-window meeting-assistant pattern seen in Perssua while keeping Koemuni's own brand, visual language and mocked behavior.
- Use an 8 px spacing rhythm: `4`, `8`, `12`, `16`, `24`, `32`, `48` px. Reserve 4 px for tight icon-to-label gaps.
- Avoid a persistent navigation rail in the compact Session window. Use 16 px outer and panel padding, with a 12 px gap between the reply card and question row.
- Expanded screens may use content padding near **24 px** and gaps between major regions near **24 px**.
- Keep primary controls visible while transcript and history content scroll independently where needed.
- Use 10–12 px corner radii for grouped panels and 7–9 px for controls. Avoid excessive pill shapes.
- Use subtle borders before shadows. Do not use shadows to separate every panel.

These dimensions are starting values for the Figma prototype, not fixed native-window constraints.

## Iconography

- Use one consistent, simple outline icon family with a 16–18 px visual size and 1.5–2 px stroke.
- Pair unfamiliar or consequential icons with a visible Portuguese label.
- Do not use emoji as interface icons. Avoid custom illustrations unless they clarify an empty state.
- Keep icons monochrome by default; use semantic status colors only when they reinforce a labeled state.

## Brand assets

The initial Koemuni identity artwork is stored in `../assets/` and shown on the Figma identity page. Keep each file separate for reuse:

- `koemuni-logo-full.png` — symbol and wordmark together.
- `koemuni-symbol.png` — canonical standalone brand symbol. Use this mark wherever the Koemuni symbol appears; do not substitute a letter “K”.
- `koemuni-app-icon.png` — the same symbol centered on a flat `#0A0A0B` square, with no rounded-square mask or additional artwork. Use as the desktop app icon and where a square app icon is needed in the Figma screens.
- `koemuni-wordmark.png` — standalone wordmark.

Treat these PNGs as visual references; they are raster artwork, not editable vector logo source.

## Components and interaction states

Create Figma components with variants for default, hover, focus, pressed and disabled states where applicable.

- **Navigation:** three destinations, “Sessão”, “Configurações” and “Histórico”. Show selection with a restrained accent treatment plus a clear label; do not rely on color alone.
- **Primary button:** filled accent for the main available action, such as “Iniciar sessão” or “Retomar”.
- **Secondary button:** neutral surface or outline for “Pausar” and other supporting actions.
- **Destructive action:** use error color sparingly for “Encerrar”; preserve a readable label and clear focus state.
- **Inputs:** visible labels, neutral dark fill, subtle border, clear focus ring and readable placeholder contrast. Placeholder text does not replace a label.
- **Transcript:** visually distinguish “Você” and “Outros” using speaker labels and layout, not color alone. Preserve line order and comfortable reading width.
- **Replies:** separate suggested answers from transcript content; retain a submitted custom question beside its answer.
- **Button alignment:** center labels horizontally and vertically in 212 × 39 px component buttons. Keep input, transcript and answer text left aligned to a shared 16 px inset.
- **Feedback:** reserve space for loading, success and error text to reduce layout jumps. Keep prior content visible on errors.
- **Demo notice:** show the exact text on every screen in a consistent, quiet banner or footer area that remains easy to find.
- **History:** make the selected session obvious and keep details read-only.

Match controls to the documented session states. Inactive, active, paused and ended states must expose only the actions allowed by [interface rules](interface.md).

## Accessibility

- Maintain readable contrast; target WCAG AA for text and controls where measurable.
- Provide visible keyboard focus on every interactive control. Use a distinct focus ring with sufficient contrast against dark surfaces.
- Use semantic control patterns, explicit form labels and a logical tab order.
- Keep interactive targets at least 32 × 32 px; prefer 36–40 px for primary controls.
- Pair status color with text or an icon and label. Do not communicate status through color alone.
- Do not move focus for routine loading or reply updates. Announce feedback politely without repeatedly announcing elapsed time.
- Respect reduced-motion preferences; transitions should be brief and never carry essential information.

## Figma file structure

Organize the design file with these pages, preserving the requested Portuguese page names:

1. **00 — Comece aqui** — product intent, visual direction and map of the file.
2. **01 — Identidade visual** — wordmark direction, personality and voice.
3. **02 — Fundamentos** — color variables, type styles, spacing, radii, borders and icon rules.
4. **03 — Componentes** — compact header navigation, buttons, inputs, status labels, demo notice, transcript rows and reply panels with states.
5. **04 — Fluxos e regras** — main flow and session-state rules.
6. **05 — Telas do MVP** — compact Session plus expanded Settings and History frames.
7. **06 — Protótipos e validação** — suggested click path and review checklist.
8. **07 — Entrega para desenvolvimento** — token map, implementation boundaries and accessibility handoff.
9. **99 — Explorações e arquivo** — archived visual options, clearly marked as unselected.

Use English names for components, variants and variables. Preserve the requested Portuguese page names and keep visible product UI text in Brazilian Portuguese. Build screens from shared components and semantic tokens so updates stay consistent.

## Screen checklist

- **Session:** compact horizontal window with status and elapsed time, session controls, latest progressive transcript line, separate suggestion and custom-question actions, and the reply.
- **Settings:** clearly fictional device choices, assistant-instructions field and a short explanation that changes last only for the current execution.
- **History:** fictional session list, selected-session details, transcript, replies and documented empty states.
- **All screens:** exact demonstration notice, visible keyboard focus in interaction examples, dark surfaces and readable long-content behavior.

## References

- [Project overview](../README.md)
- [MVP requirements](mvp.md)
- [Interface and interaction rules](interface.md)
- [Architecture and stack](architecture.md)
- [Implementation backlog and manual review](backlog.md)
