# Interface prototype

## Problem and objective

Meeting participants need to follow the conversation and find a useful way to respond. This phase tests whether the proposed interface makes that workflow clear, readable and easy to control.

Validate usability with fictional conversations and predefined answers. Transcription accuracy, answer relevance and AI quality require a later phase with a separately agreed scope; this prototype provides no evidence about them.

## Main flow

Open Session → start a simulated session → read progressive transcript lines → request a suggestion or ask a custom question → pause and resume → end → open its details in History. Settings may be changed without leaving or resetting the session.

## Included scope and acceptance criteria

Each numbered requirement is accepted only when its observable criteria pass.

| ID | Functional requirement | Observable acceptance criteria |
| --- | --- | --- |
| FR-01 | Navigate between Session, Settings and History. | All three screens are reachable by keyboard; switching screens preserves in-memory session and settings state. Every screen visibly shows “Modo demonstração — nenhum áudio está sendo capturado”. |
| FR-02 | Start, pause, resume and end a simulated session. | Start creates one active session. Status and elapsed active time are visible. Pause freezes duration and transcript arrivals; resume continues both without duplication. End freezes the final duration and prevents further arrivals. Invalid actions are unavailable. |
| FR-03 | Show a progressive fictional transcript. | Lines appear in fixture order only while active, labeled “Você” or “Outros”. A new session starts with an empty transcript. Exhausting the fixture stops arrivals while the session remains active until paused or ended. |
| FR-04 | Suggest a predefined reply. | “Sugerir resposta” shows loading, prevents duplicate requests and then displays a predefined answer. Pausing or ending during loading cancels the pending result. A deterministic simulated failure can be triggered and retried without losing transcript data. |
| FR-05 | Answer a custom question with simulated content. | Blank or whitespace-only input cannot be submitted. A nonempty question shows loading, then its predefined answer alongside the submitted question. The same cancellation, duplicate-request and simulated-error rules as FR-04 apply. No model or API is called. |
| FR-06 | Edit demonstration settings. | Fictional microphone and audio-output choices and assistant instructions are editable and preserved across navigation. Devices are clearly fictional; instructions do not change predefined replies. Reload restores fixture defaults. |
| FR-07 | Browse fictional session history and details. | Initial fixture sessions are listed. Selecting one shows its transcript and suggestions. An empty list and a session with no suggestions have explanatory empty states. |
| FR-08 | Add ended sessions to history in memory. | Ending an active or paused session adds exactly one entry with its final duration, transcript and completed suggestions. Ending again cannot duplicate it. Starting another session does not change previous entries. Reload removes newly added entries and restores fixtures. |

## Excluded scope

- Real audio or screen capture, microphone permissions and real transcription.
- AI APIs or models, external integrations, login, users and billing.
- Remote backend, Axum, PostgreSQL, SQLite and any persistence, including browser storage and files.
- Global shortcuts, hiding from screen sharing and always-on-top behavior.
- Deployment, updater and installer distribution.

## Definition of done

- FR-01 through FR-08 pass a recorded manual walkthrough, including loading, empty and simulated-error states.
- Interface text is Brazilian Portuguese; focus is visible, controls have labels and the complete flow works by keyboard.
- Timers and pending replies cannot mutate a paused or ended session, a different session or a disposed owner.
- Available typecheck and build checks pass; justified state/timer tests pass. Record actual commands, results and any unavailable checks.
- No real capture, permissions, network service or persistence is introduced. Rust stays within Tauri bootstrap.
- Documentation matches implemented behavior. Describe target-environment validation only after actually running it; record any untested environment as a limitation.

Usability review observes whether a person can recognize demonstration mode, follow the transcript, request help and locate an ended session without guidance. Predefined replies validate interaction only, regardless of how convincing their wording seems.
