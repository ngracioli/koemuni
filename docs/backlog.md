# Implementation backlog

All items are pending. Each item should produce a small reviewable change. This checklist does not authorize implementation during the documentation task.

| Done | Delivery | Expected result | Validation |
| --- | --- | --- | --- |
| [ ] | 1. Scaffold | Tauri 2 native bootstrap with Vue 3, Composition API, `<script setup>`, TypeScript, Vite and Tailwind CSS; no custom native integration. | Inspect actual generated scripts; run available typecheck and build checks. Record results and native prerequisite limitations. Document only commands that now exist. |
| [ ] | 2. Layout | Three navigable screens, dark compact layout, Portuguese labels and permanent demonstration notice. | Keyboard walkthrough of navigation, focus, labels, contrast and long-content scrolling. Run available typecheck/build checks. |
| [ ] | 3. Session simulation | Fictional transcript, start/pause/resume/end controls, active duration and timer cleanup. | Walk through FR-02/03, navigation while active and fixture exhaustion. Add focused state/timer tests for pause/resume, cancellation and disposal; run typecheck/build. |
| [ ] | 4. Replies | Predefined suggestion and custom-question response, loading, deterministic error and retry. | Verify FR-04/05, blank-input blocking and cancellation on pause/end/new session. Extend timer tests only for pending-result races; run typecheck/build. |
| [ ] | 5. Settings | Fictional device selectors and editable assistant instructions, retained in memory. | Change values, switch screens and reload; verify FR-06 and absence of device access or persistence. Run typecheck/build. |
| [ ] | 6. History | Fixture list/details and one snapshot per ended session. | Verify FR-07/08, empty states, immediate ending, duplicate prevention and independence from the next session. Run typecheck/build and relevant state tests. |
| [ ] | 7. Final review | Consistent documented prototype, with limitations recorded. | Complete the manual script below; run available typecheck/build and justified tests. Record actual target-environment execution or explicitly mark it untested. |

## Manual review script

1. Open each screen by keyboard; verify visible focus, labels and the exact demonstration notice. Confirm inactive transcript/reply states and initial fictional history.
2. Start a session. Observe ordered lines from “Você” and “Outros” and advancing duration. Navigate away and back; confirm continuity without duplicate arrivals.
3. Pause and wait beyond a fixture delay: duration and transcript remain unchanged. Resume; confirm one continuation. Let the fixture finish; duration continues while active.
4. Request a suggestion and submit a custom question. Confirm loading and predefined answers. Try blank input, duplicate submission, the deterministic error scenario and retry.
5. Pause or end while a reply is loading. Wait beyond its delay; confirm no late answer. Start another session and verify old callbacks cannot affect it.
6. End active and paused sessions, including one before its first line. Confirm one history entry per session with final duration and completed content. Open details; start another session and confirm old details stay unchanged.
7. Change settings and navigate between screens. Reload: settings and newly added history reset to fixtures. Use an empty-history fixture to check empty states; check details without suggestions.
8. Dispose the session owner in the focused timer check and confirm no pending updates. Inspect implementation for unintended capture, permissions, network integrations and persistence.

Prioritize typecheck, build and manual usability review. Automated tests are justified for state transitions and timer/cancellation behavior; avoid adding a broad test framework or tests that only mirror static markup. If no appropriate test tooling exists, a small executable assertion check is sufficient for that logic.

No command names or successful results are assumed before scaffolding. A frontend build alone does not establish native compatibility on Arch Linux, KDE Plasma or Wayland.
