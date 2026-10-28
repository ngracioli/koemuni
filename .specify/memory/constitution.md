<!--
Sync Impact Report
- Version change: template scaffold → 1.0.0 (initial ratification)
- Modified principles: none
- Added sections: Koemuni Core Principles, Product and Technical Boundaries,
  Delivery and Quality Standards
- Removed sections: template examples and placeholders
- Follow-up TODOs: none
-->

# Koemuni Constitution

## Core Principles

### I. Validate the Interface Within the Agreed Phase
The current product phase is a mocked desktop interface prototype. Work MUST
validate navigation, readability, meeting controls and the fictional session
flow using in-memory Vue state and fictional data. The prototype MUST NOT be
presented as evidence of transcription accuracy, answer relevance or AI quality.
Every screen MUST display “Modo demonstração — nenhum áudio está sendo capturado”.

### II. Keep Product Scope Explicit
Implementation MUST follow the accepted MVP requirements and exclusions. The
prototype MUST NOT add real audio or screen capture, microphone permissions,
transcription, AI models or APIs, authentication, billing, remote services,
persistence, external integrations, global shortcuts, screen-sharing concealment,
always-on-top behavior, deployment, updates or installer distribution. New
capabilities require a separately agreed scope before implementation.

### III. Keep State Local and Behavior Predictable
Prototype data MUST remain fictional and in memory; reload or application close
MUST restore fixture defaults and discard session changes. Vue primitives and
composables MUST own frontend state; add no global state manager or service layer
without an accepted requirement. Session transitions, timers and pending replies
MUST not mutate paused, ended, replaced or disposed sessions. Simulated replies
MUST use predefined local content and MUST NOT make network calls.

### IV. Make the Interface Accessible and Consistent
Visible product text MUST be in Brazilian Portuguese. Controls MUST have labels,
keyboard navigation and visible focus. Status MUST not rely on color alone, and
long transcripts or replies MUST remain readable without hiding primary controls.
All screens MUST retain the exact demonstration notice. UI changes MUST follow
the repository's Figma-first workflow and reuse suitable shared components.

### V. Prefer the Smallest Maintainable Change
Use the established stack: Tauri 2 bootstrap, Rust limited to generated native
startup, Vue 3 Composition API with `<script setup lang="ts">`, TypeScript, Vite
and Tailwind CSS. Changes MUST follow existing ownership and naming conventions,
avoid unnecessary dependencies and abstractions, and stay within the current
delivery. Handwritten functions MUST have cyclomatic complexity at most 10 and
control-flow nesting at most three levels. Do not change system configuration.

## Product and Technical Boundaries

The initial target is Arch Linux, KDE Plasma, Wayland and PipeWire; compatibility
is unverified until exercised on that environment. A frontend build MUST NOT be
reported as proof of native runtime compatibility. Proposed screen dimensions,
visual direction and folder structure are starting points, not fixed product
requirements. Rust remains bootstrap-only for this phase.

## Delivery and Quality Standards

Before implementation, contributors MUST read the project instructions and the
MVP, interface, design system, architecture and backlog documents. They MUST
inspect the working tree and preserve existing user changes. Each delivery MUST
stay within its defined scope and exit criteria. Documentation MUST be updated
when behavior changes and MUST remain in English, except intentional Portuguese
product examples and original external asset or Figma names.

Implementation changes MUST run applicable frontend typecheck/build and other
quality gates defined in the architecture documentation. State and timer changes
MUST receive focused deterministic checks when justified by the behavior.
Documentation-only changes require link, consistency and whitespace checks.
Reports MUST state actual commands and results, unavailable checks, and
environment limitations; they MUST NOT claim unverified compatibility. The
manual MVP walkthrough MUST cover the acceptance criteria before the prototype
is declared complete.

## Governance

This constitution governs project decisions together with the repository's
`AGENTS.md` and product documentation. If a proposed change conflicts with these
rules, contributors MUST resolve the conflict in the relevant project artifact
and obtain agreement on changed scope before implementation. Review MUST check
scope, demonstration-mode disclosure, accessibility, state ownership and the
applicable quality gates.

Amendments require an explicit project decision and MUST update this document's
version and last-amended date. Use semantic versioning: MAJOR for incompatible
governance changes, MINOR for new principles or materially expanded constraints,
and PATCH for clarifications that do not change requirements. Keep product
requirements and delivery details in their canonical documentation; this
constitution records durable project rules.

**Version**: 1.0.0 | **Ratified**: 2026-10-28 | **Last Amended**: 2026-10-28
