# Interface proposal

Visual direction and component boundaries below are initial proposals, adjustable through usability review within the mocked scope. All visible text must use Brazilian Portuguese.

## Window model

Keep Session as a low-height horizontal utility window by default. The Figma reference is 920 × 390 px, without a persistent sidebar; show the session state, elapsed time, lifecycle controls, latest transcript line, reply actions, custom question and demo notice together. Open Settings and History in larger secondary windows (currently 1200 × 800 px in Figma). Moving between screens must preserve the in-memory session and settings state.

## Shared layout and screens

Use a compact near-black desktop layout with purple as the primary accent, readable text and consistent spacing. Use an italic treatment for the latest transcript excerpt and a subtle purple outline for suggested answers. Preserve contrast and allow long transcripts and answers to scroll without hiding primary controls. Avoid conveying status through color alone.

Keep navigation (“Sessão”, “Configurações”, “Histórico”) and the visible notice **“Modo demonstração — nenhum áudio está sendo capturado”** on every screen.

| Screen | Main components and content |
| --- | --- |
| Sessão | Session status and duration, start/pause/resume/end controls, transcript with “Você” and “Outros”, suggestion action, custom-question input and completed replies. |
| Configurações | Labeled fictional microphone and audio-output selects, assistant-instructions textarea and explanation that choices last only for the current execution. Changes apply directly to memory; no save service or device enumeration. |
| Histórico | Fictional and newly ended session list; selected entry details with duration, transcript and suggestions, plus a return-to-list action. Details are read-only. |

Proposed components: application shell, session controls, transcript list, reply panel, settings form and history list/details. Reuse native labeled form controls where sufficient.

## Session transitions and controls

`inactive → active ⇄ paused → ended`; an active session may also end directly. Starting from ended creates a fresh active session with a new identity and empty content. Navigation does not change session status.

| State / visible label | Available controls | Unavailable controls |
| --- | --- | --- |
| Inactive / “Inativa” | “Iniciar sessão” | Pause, resume, end and reply requests. |
| Active / “Em andamento” | “Pausar”, “Encerrar”, “Sugerir resposta”, custom-question submit when nonblank | Start and resume; both reply actions while a reply is pending. |
| Paused / “Pausada” | “Retomar”, “Encerrar” | Start, pause and reply requests. |
| Ended / “Encerrada” | “Nova sessão”; history navigation | Pause, resume, end and reply requests. |

Hide irrelevant lifecycle controls or disable them consistently. Preserve completed transcript lines and replies when paused or ended. Duration counts active time only; ending archives exactly once, even when no lines have arrived. A new session resets the question, request state and current transcript without changing history or settings.

## Feedback states

| Area | Empty | Loading | Success | Simulated error |
| --- | --- | --- | --- | --- |
| Transcript | “Inicie uma sessão para ver a transcrição simulada.”; active before first line: “Aguardando falas simuladas…” | Waiting for next scheduled fixture line; no capture indicator | Append labeled lines in order | No transcript failure simulation required. |
| Replies | “As sugestões aparecerão aqui.” | “Preparando resposta simulada…”; disable both submission actions | Show predefined answer; keep custom question beside its answer | “Não foi possível gerar a resposta simulada. Tente novamente.”; allow retry while active. |
| History | “Nenhuma sessão no histórico.”; details without replies: “Nenhuma sugestão nesta sessão.” | No artificial loading for in-memory data | Show list and selected details | No history failure simulation required. |
| Settings | Fixture defaults; instructions may be empty | No loading for local edits | Selected values remain visible across navigation | No device or permission errors: no device access occurs. |

Use a deterministic fixture scenario for one reply failure followed by a successful retry. Keep that scenario selectable through fixture data for validation; no extra settings screen or random failures. Preserve entered question and earlier content on error. Pause/end cancels pending loading and clears its feedback without appending an error or late answer.

## Timers and pending work

- Keep session simulation in an app-owned composable so screen navigation does not stop an active session or create duplicate timers.
- Pausing clears transcript/duration timers and cancels pending reply delays. Preserve active elapsed time and remaining delay to the next transcript line; resume schedules that remaining delay once. Canceled replies require a new request after resume.
- Ending clears every session timer and pending reply, then stores a snapshot of completed content. No callback may append content afterward.
- Starting another session invalidates old callbacks. Check session identity and status before applying delayed results, including callbacks already queued when canceled.
- Disposing the owning composable/component clears all timers and invalidates pending work. Screen components clean up their own effects on unmount; the shared session owner lives until the application root unmounts.
- Derive duration from accumulated active time and the current active interval, rather than counting timer ticks. Fixture exhaustion stops transcript scheduling only.

## Basic accessibility

Use semantic buttons, visible focus, explicit form labels and logical tab order. All actions must work by keyboard without global shortcuts. Move focus to a useful heading when opening history details and back to the selected entry on return. Announce reply loading, success and error politely without moving focus or repeatedly announcing the duration. Keep transcript reading and scrolling under the user's control.
