# GitHub request: supported read-only status access for GitHub Copilot app sessions

| Field | Value |
|---|---|
| Status | **Approved — ready for Guy to send.** Worf privacy review: approved with changes (2026-11-08). Picard technical review: rejected (2026-11-08); Geordi revision applied (2026-11-08); Picard re-review: approved (2026-11-08). Next: Guy chooses a channel, sends, and fills the send record. |
| Owner | Data (drafts and evaluates); Worf (privacy review); Picard (technical approval and gate disposition); Guy Bertental (@guybartal) chooses channel and sends |
| Related plan task | `P01-T01` — Obtain and evaluate the supported external status contract (request items 1–5) |
| Decision | `D9` = A — Guy sends the prepared request; product work stays gated until the response is evaluated |
| Drafted | 2026-11-08 |
| Sent | Not sent. No channel selected. |

Internal notes (not for sending): only the section **Outgoing message** is sent. All linked URLs are public pages already verified in the status-access research (W2, W3, W8, W9, W17, W19, W20, W23, W25). The message asks questions only; it does not state or imply that GitHub has promised anything. Channel options from the plan: GitHub Support ticket, GitHub Community discussion, or the public `github/copilot-sdk` issue tracker.

---

## Outgoing message

**Suggested title:** Supported read-only way to read GitHub Copilot app session status on macOS?

**Body:**

Hello GitHub team,

I'm building an open-source, monitoring-only Apple Watch companion for the GitHub Copilot app on macOS. The first release reads status on a single Mac. It would show each session as one of four states: **Working**, **Needs you**, **Finished responding**, or **Unknown**. It never answers prompts, approves plans, or starts work, and it needs no prompts, responses, code, or file contents.

I want to use only supported, terms-compliant mechanisms, not private stores, undocumented endpoints, scraping or reverse engineering. I've read these public docs and found useful building blocks, but no documented contract for this use:

- Canvas extensions: https://docs.github.com/en/copilot/how-tos/github-copilot-app/working-with-canvas-extensions
- Hooks: https://docs.github.com/en/copilot/concepts/customizations/hooks and https://docs.github.com/en/copilot/reference/hooks-reference
- SDK session events: https://raw.githubusercontent.com/github/copilot-sdk/main/docs/features/streaming-events.md
- Experimental peer listing (`session.management.list`): https://github.github.io/copilot-sdk-java/1.0.29-preview.9/apidocs/com.github.copilot.java/com/github/copilot/generated/rpc/SessionManagementApi.html
- Sessions and Projects: https://docs.github.com/en/copilot/how-tos/github-copilot-app/agent-sessions and https://docs.github.com/en/copilot/how-tos/github-copilot-app/working-with-projects

Could you answer the following?

1. **Support and use.** Is there a supported way for a locally installed third-party component to read status for all existing Copilot app sessions on macOS, including already-open and already-waiting sessions in chats and Projects? Which app/runtime versions, and is it stable or experimental? May an independent third party build and publicly distribute an open-source client that uses it, and under what terms?
2. **Sanctioned route.** Which, if any, is intended for this: (a) a canvas extension observing sessions it wasn't launched in, while the user works elsewhere; (b) desktop hooks, and which events/fields apply to the app; (c) `session.management.list` for app sessions, and under what opt-in; (d) delivery of SDK events (`user_input.requested`, `elicitation.requested`, `exit_plan_mode.requested`, `permission.requested`, `session.idle`) for existing app sessions?
3. **Minimal fields.** Is there a stable session ID and its mapping to app session, runtime session, Project and task IDs; the session name and Project name; a waiting category distinguishing question/elicitation, plan approval and permission; a source state-entry timestamp distinct from event or observation time; and a current state that distinguishes working, waiting (by those categories), response ended, cancelled or errored, and unknown, so an ended response is not mistaken for an aborted one?
4. **Lifecycle.** Is there a current-state snapshot that includes already-pending waits; transition ordering; a stable identifier on each transition usable for de-duplication; replay or resync after missed events; a heartbeat or freshness signal; defined behaviour across sleep, crash, restart and reconnect; and the expected delay between a state change in the app and the status becoming readable through the route (our end-to-end alert target is 30 seconds)?
5. **Permission and privacy.** Is read-only, least-privilege access available that cannot answer, approve, prompt, start/resume work or change tool/permission settings? How do user consent, admin policy and revocation work? Can status be obtained without receiving prompts, responses, plan content, code or files, i.e. excluded at the source rather than delivered and filtered by our client?

If no supported mechanism exists today, a clear "not supported" answer is equally helpful. Thank you!

Guy Bertental (@guybartal)

---

## Evaluation rubric (internal — do not send)

Qualifying evidence: official GitHub documentation, a published API reference, or a written response through an official GitHub channel, for named versions. Evaluate each item, then the whole, against the `P01-T01` gate.

| Item | Pass | Partial | No-go |
|---|---|---|---|
| 1. Support and use | Named supported mechanism, versions and stability; covers all existing app sessions (chats and Projects, incl. already-waiting); third-party use permitted for the intended distribution (public open-source client, D5; one Mac in R1, D2) | Supported but experimental, version-limited, opt-in per session, subset of sessions, or distribution terms unclear | GitHub states no supported mechanism, or third-party use is not permitted |
| 2. Sanctioned route | One route explicitly sanctioned for passive observation of existing app sessions, running in the background | Route works only for sessions it launched/joined, only in foreground, or event delivery to app sessions is partial | Only route needs private stores/endpoints, scraping, reverse engineering or credential extraction |
| 3. Minimal fields | Stable ID with mapping, session name, Project name, wait category (question vs plan approval vs permission), source state-entry timestamp, and a current state distinguishing working, waiting (by category), response ended, cancelled/errored and unknown | Some fields missing (e.g. no state-entry timestamp, no Project name, wait category not distinguished, or response ended not distinguishable from cancelled/errored or unknown) → per-item gap list routed to a user-owned PRD revision decision | Status unobtainable without content-bearing payloads (see item 5) |
| 4. Lifecycle | Snapshot incl. pending waits, ordering, stable per-transition identifier for de-duplication, replay/resync, freshness signal, defined sleep/crash/restart/reconnect behaviour, and a stated change-to-readable delay compatible with the 30-second p95 alert target (`NFR-001`) | Events only, no snapshot or replay; no transition identifier; delay unstated or too long for `NFR-001`; freshness or restart behaviour undefined → gap list | — (gaps here are partial unless combined with no-go elsewhere) |
| 5. Permission and privacy | Read-only least-privilege access with consent and revocation; no prompt/response/plan/code/file content required | Read-only but content-bearing payloads that require local filtering, or consent/revocation undocumented → Worf review and gap list | Only route requires write/control privileges or content export |

Overall disposition (per plan):

- **Pass (contract):** items 1–5 covered by qualifying evidence → seek separate authorization for minimized runtime verification; `P01-T01` checked only after verification reproduces required states.
- **Partial:** some items covered → per-item gap list; each gap routed to a user-owned PRD revision decision (e.g. duration from observation instead of state entry, per F5); task stays unchecked.
- **No-go:** GitHub states no supported mechanism, or the only route needs private access, content export or write/control privileges → record explicit no-go; `D9` contingency governs; `P02` does not start for the original scope.
- **No response or unclear:** remains blocked; `D9` governs any timebox; neither pass nor no-go assumed.

Evidence that never counts: observing the app UI; agent-visible tools such as `get_sessions_status`; generated declarations alone; community projects; a shared database path; the app being built on the CLI. Also excluded: idle interpreted as success, and poll-observation time silently substituted for source-transition time.

---

## Review record

### Worf — privacy review

- Reviewer: Worf (Quality & Security Engineer)
- Date: 2026-11-08
- Scope checked: (a) no private repo content, internal paths, secrets or personal data beyond Guy's name/handle, which he chooses to post — pass; all links are public. (b) item 5 least-privilege/consent/revocation/no-content asks — strengthened. (c) CON-004 — no implied use of internals; explicit statement added. (d) D5/D2 context — added.
- Outcome: **Approved with changes**
- Notes / edits made:
  1. Intro: "personal" → "open-source" (D5); added single-Mac first-release scope (D2).
  2. Added explicit exclusion of private stores, undocumented endpoints, scraping and reverse engineering (CON-004).
  3. Item 1: distribution question now asks explicitly whether an independent third party may publicly distribute an open-source client using the route, and on what terms (D5); App Store wording dropped as out of current decision scope.
  4. Item 5: "user consent"; added ask whether content can be excluded at the source rather than delivered and filtered by the client (NFR-007).
  5. Rubric item 1 Pass cell annotated with D5/D2 only; rubric otherwise unchanged.
  - Residual: Guy should post from a channel that doesn't auto-attach diagnostics/logs (e.g. Support ticket system info) and attach nothing else. Experimental `session.management.list` is referenced as a question only, not a commitment to use it.

### Picard — technical approval

- Approver: Picard (Lead & Architect)
- Date: 2026-11-08
- Outcome: **Rejected — changes requested.** Not approved to send.
- Checked against: `P01-T01` request items 1–5 and gate evaluation (pass / partial / no-go / no response) in [.copilot-tracking/plans/2026-11-08/agenticwatch-mvp-plan.md](../../plans/2026-11-08/agenticwatch-mvp-plan.md).
- What passes: item 1 (all existing app sessions incl. already-open/already-waiting, chats and Projects; versions; stability; open-source third-party distribution per D5; one Mac per D2); item 2 (canvas extension on sessions it did not launch and in the background, desktop hooks, `session.management.list` opt-in, SDK event delivery); item 3 identity mapping, session and Project names, wait category, distinct source state-entry timestamp; item 4 snapshot incl. pending waits, ordering, replay/resync, freshness, sleep/crash/restart/reconnect; item 5 in full, including Worf's source-side content exclusion. Worf's privacy edits, `CON-004` statement and public-only links are accepted. The rubric's four overall dispositions match the plan.
- Why rejected: the outgoing message omits three asks that `P01-T01` items 3 and 4 require. Without them a reply could meet the rubric's Pass cells while leaving the four honest labels or the 30-second p95 path unknown, forcing a second vendor round-trip of unknown duration.
- Required changes (all three; nothing else in the outgoing message changes):
  1. **Item 3 — full state set.** Add a question asking whether the mechanism exposes a current state that distinguishes *working/busy*, *waiting* (by the categories already asked), *response ended*, *cancelled or errored*, and *unknown*, so an ended response is distinguishable from a cancelled or aborted one. (Plan item 3; `FR-002`, `NFR-010`; research F3 notes `session.idle` can also mark an aborted turn.)
  2. **Item 4 — transition identity.** Add a question asking whether each state transition or event carries a stable identifier usable to deduplicate across retries, restarts and reconnects (in addition to the existing "transition ordering"). (Plan item 4; `FR-009`, `AC-009`.)
  3. **Item 4 — cadence/latency.** Add a question asking for the expected cadence or latency between a state change in the app and its availability through the mechanism. (Plan item 4; `NFR-001`, `NFR-003`.)
  4. **Rubric alignment (internal section).** Add the item 3 state set to rubric row 3 Pass (missing distinction → Partial gap), and transition identity plus cadence/latency to rubric row 4 Pass (missing → Partial gap). No other rubric change.
- Constraints on the revision: questions only; no new links unless already in the research evidence; no promise or implication of GitHub commitment; keep D2/D5 context and Worf's item 5 wording unchanged; no App Store/TestFlight wording (that channel sub-question is unresolved — plan `D10`).
- Revision owner: **Geordi** (platform delivery/latency context; not an author). Data (original author) and Worf (edited the outgoing message) are locked out for this revision cycle per the reviewer protocol. Worf may re-check only the privacy impact of the changed text as reviewer, not co-author it.
- Next: Geordi revises → Picard re-review (approve or reject) → Worf privacy re-check → Guy chooses channel and sends. Status then moves to "Approved — ready for Guy to send".

### Revision (Geordi)

- Reviser: Geordi (Apple Watch & iPhone Engineer), independent of the original author under the reviewer lockout
- Date: 2026-11-08
- Changes (only those Picard required):
  1. Item 3: added a question on whether the current state distinguishes working, waiting (by category), response ended, cancelled or errored, and unknown, so an ended response is not mistaken for an aborted one.
  2. Item 4: added a question on a stable per-transition identifier usable for de-duplication.
  3. Item 4: added a question on the expected delay between an app state change and the status becoming readable through the route, citing the 30-second end-to-end alert target.
  4. Rubric rows 3 and 4: Pass now requires the state set (row 3) and transition identifier plus a delay compatible with `NFR-001` (row 4); missing items are listed as Partial gaps. No other rubric change.
- Unchanged: intro (D5 open-source, D2 one Mac), `CON-004` statement, public-only links, items 1, 2 and 5 (Worf's wording), sign-off. No new links, no commitment implied, no App Store/TestFlight wording.
- Next: Picard re-review → Worf privacy re-check of the changed text → Guy sends.

### Picard — technical re-review

- Approver: Picard (Lead & Architect)
- Date: 2026-11-08
- Verdict: **Approved.** Ready for Guy to send.
- Notes:
  1. Item 3 now asks for a current state distinguishing working, waiting (by category), response ended, cancelled or errored, and unknown, and says why (ended vs aborted) — satisfies required change 1.
  2. Item 4 now asks for a stable per-transition identifier for de-duplication alongside ordering, and for the expected delay from app state change to readable status against the 30-second end-to-end target — satisfies required changes 2 and 3.
  3. Rubric rows 3 and 4: Pass cells require the state set, transition identifier and an `NFR-001`-compatible delay; missing items are Partial gaps — satisfies required change 4. Overall dispositions unchanged and still match the plan.
  4. Preserved: D5 open-source and D2 one-Mac intro, `CON-004` statement, Worf's item 1 and item 5 wording, public-only links (no new links), no implied GitHub commitment, no App Store/TestFlight wording.
  5. Length 481 words incl. title/URLs (~480 target); clarity is fine.
  6. The changed text adds only metadata questions (no content, links or personal data), so Worf's privacy approval stands; a Worf re-check is advisory and does not block sending. Worf's residual note (post from a channel that does not auto-attach diagnostics; attach nothing) still applies.

### Send record (Guy)

- Channel: _pending_
- Date sent: _pending_
- Link/reference: _pending_
