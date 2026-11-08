<!-- markdownlint-disable-file -->
# Task Research: AgenticWatch supported desktop session status access

| Field | Value |
|---|---|
| Date | 2026-11-08 |
| Researcher / agent | Data — Copilot Integration Engineer; rpi-research |
| Output mode | analysis; evidence for planning, not architecture selection |

## Executive Summary

**ASM-001 remains unproven: no sufficient vendor-supported, read-only external contract for existing macOS Copilot desktop sessions was verified. Product implementation and the plan's P02 design gate must remain Not ready.** This is a bounded missing-evidence result, **not** proof that no such integration exists.

The investigation is complete within the authorized public-source scope. Official current documentation confirms desktop hooks and canvas extensions; public SDK contracts contain session events and experimental live-peer status. These are credible leads, but do not prove the required desktop session coverage, permission boundary, project/name mapping, source state-entry times, or recovery guarantees.

Confidence is high in what the fetched contracts say and in the remaining gaps; confidence in an exhaustive negative would be unjustified. The smallest missing evidence is a GitHub-supported desktop status endpoint **or extension/event contract** that explicitly covers existing app sessions, permissions, schema, lifecycle/freshness and read-only operation. No unsupported fallback, alternative product, runtime probe or architecture was selected.

## What You May Not Know

* **Desktop customization is real.** GitHub documents hooks and canvas-based session monitoring. "No desktop integration exists" would overstate the evidence (F2).
* **The SDK is generally available as of June 2, 2026**, not universally technical preview. Individual peer-management, canvas-wire and AHP-hosting APIs remain explicitly experimental (F2, F3).
* **Experimental SDK APIs can list opted-in existing live local peers as busy/idle.** They are not limited to creating new sessions. That still does not establish all-existing-desktop access (F2).
* **SDK plan-approval events exist.** The unresolved issue is their supported delivery for desktop sessions and recovery, not universal absence of a plan event (F3).
* A local desktop session can sync to GitHub, but sync is not a documented metadata-only live-status feed. A repository is also not necessarily a desktop **Project**, which may span repositories or contain none (F3, F4).

## Findings

### F1 — External desktop access is not yet proven, not proven impossible

The checked official pages establish a macOS desktop product, session modes, first-party monitoring, hooks, canvases, sync, and navigation. They do **not**, in the fetched material, provide one sufficient third-party contract for listing existing app sessions and observing the requested states with read-only permissions, timestamps and recovery.

* Questions: Q1, Q5.
* Evidence state: **Unresolved possibility** — ASM-001 remains unproven.
* Evidence: C1–C2 define the gate; W1–W5, W7–W9, W13, W19, W22 describe current desktop features; F2–F4 preserve the strongest candidate routes and their limitations.
* Confidence and limits: High confidence that this evidence is insufficient for the gate; no exhaustive-absence claim. Live docs and recent release notes were followed beyond search results, including a newer navigation path for the desktop concept page. Unpublished, unindexed or newly changed vendor contracts may exist.

The conversation's `get_sessions_status` and related tools are **app-authorized agent access**. Their presence does not grant a separately installed consumer permission, a stable endpoint, an export schema, a support promise, or a lifecycle contract. They were not invoked as a product integration test. An MCP server exposing a similarly named cloud tool would likewise need documented applicability, not a name match.

### F2 — An app-resident extension/hook route is plausible but incomplete

GitHub documents desktop repository/plugin hooks and canvas extensions. The canvas page even lists "launching and monitoring agent sessions" as an example. Public GitHub canvas authoring material shows `joinSession` and `session.on`. These are legitimate public customization leads, not private APIs discovered by inspection.

* Questions: Q1, Q3.
* Evidence state: **Partially supported claim** — an app-resident metadata bridge is plausible; a sufficient supported desktop contract is not established.
* Evidence: W2–W4, W7–W8, W11–W12, W16, W20, W22–W25.
* Confidence and limits: High that the documented desktop surfaces exist; medium that they could support the full monitor. Public generated definitions and authoring examples are not a blanket desktop deployment/support guarantee.

| Candidate | What is actually documented | Missing link for this MVP |
|---|---|---|
| Desktop canvas/extension | Repository/user canvas installation; shared UI/actions; public authoring shows host-forked per-session `joinSession`, runtime events and RPC | Supported passive startup/attachment to every already-open desktop session; catalog including chats/projects; app naming; minimal read-only privilege; background lifecycle/revocation and exact status parity |
| Desktop hooks | Desktop supports repository hooks and plugin-supplied hooks | The detailed hooks reference explicitly scopes its event/input/output schema to CLI/cloud. The concepts page says not to assume identical behavior across surfaces. Desktop event matrix, already-waiting snapshot, reload behavior and app-level plan waits remain missing |
| Experimental SDK `session.management.list` | Lists **other live opted-in local peers**, within an eligible configuration-directory/process boundary; owner runtime reports `sessionId`, `name`, `cwd`, idle/busy, optional model/parent ID | Master switch is off by default; desktop activation/eligibility not established. No question/plan-wait category, source state-entry timestamp, project name, all-session guarantee, replay or heartbeat contract in this result |
| SDK runtime URI / experimental AHP hosting | Connect to an already-running runtime with connection-token option; host an SDK-owned AHP catalog | No supported discovery/permission path to the running desktop app shown. Starting a new host does not adopt arbitrary existing app sessions; no probing of listeners/tokens was attempted |

`joinSession` in the pinned public SDK source rejects callers without a host-provided session identity and uses the parent-process connection. This is **not** an arbitrary external desktop attach API. The documented CLI extension lifecycle reloads on foreground replacement and ends on CLI exit. Those details cannot be silently promoted to a whole-desktop, always-on monitor contract.

The peer-management lead weakens any assertion that "SDK can only see sessions it creates," but its restricted/experimental catalog does not prove ASM-001. The canvas host context in the checked generated schema advertises canvas rendering capability, not an app-wide status method. Neither public installation support nor an available `session.rpc` property proves every generated RPC is permitted for an extension in the desktop host.

### F3 — Useful event schemas exist, but desktop mapping and recovery remain missing

The SDK documents events with unique event IDs and ISO 8601 event-creation timestamps. Root versus subagent attribution is explicit. The pinned generated event schema also defines plan-approval request/completion events. These could underpin precise monitoring **if** GitHub establishes supported desktop delivery, snapshots and permission.

* Questions: Q2, Q3.
* Evidence state: **Evidence-backed finding** — rich SDK event/schema capabilities exist; their desktop applicability is unresolved as stated in F1–F2.
* Evidence: W10, W17, W20, W23; C1–C2 require honest state and source timing.
* Confidence and limits: High for the published SDK schema. No runtime behavior, desktop event parity or end-to-end benchmark was tested. A generated declaration is evidence of a contract shape, not proof of a shipping desktop capability.

| Required observation | Verified neighboring contract | Existing desktop status coverage / limit |
|---|---|---|
| Stable session identity | SDK catalog `sessionId`; events `id`, request `requestId`; app deep links distinguish app-local/session/task IDs | No verified mapping among app session, runtime session, project and GitHub task IDs |
| Session display name | SDK `summary`, title-change event; experimental local metadata `name` and peer catalog `name` | Exact current app name and rename propagation not promised |
| Project name | SDK repository/cwd context; desktop Projects may span repositories or none | Repository/cwd cannot be relabeled project name without contract |
| Working / busy | `assistant.turn_start`; experimental `hasActiveWork`; peer catalog says idle/busy | App-level active-work semantics, suspended waits and background-work inclusion require verification |
| User question / structured input | `user_input.requested/completed`, `elicitation.requested/completed` | Events include question/message/schema/answer; metadata-only reduction must happen locally. Already-pending desktop snapshot and supported extension visibility not established |
| Plan approval | `exit_plan_mode.requested/completed`; request has `requestId`, plan content/summary/actions; completion has decision and request ID | **Events exist**, but are ephemeral. App plan tool/approval parity, initial pending snapshot and read-only subscription remain unproven |
| Tool permission wait | `permission.requested/completed`; CLI notification `permission_prompt` | Different from plan approval or asking a question. CLI `permissionRequest` hook fires before automatic permission decisions, so firing alone is not proof that the user is blocked |
| Finished responding | SDK `session.idle` says processing is done; includes optional `aborted`; CLI `agentStop` is a turn-stop hook | Never infer task success. Stop hooks can force continuation; subagent idle/end and shell completion are not parent response completion |
| State-entry time / duration | SDK event `timestamp` is event creation; CLI hook `timestamp` is hook time | No source-authoritative desktop state-entry timestamp verified. Catalog `modifiedTime`/cloud `updated_at` are not state-entry times |
| Freshness / disconnect | SDK runtime `ping`; CLI remote metadata optionally `staleAt`; first-party remote control requires online host | Not an app-session liveness/snapshot-age guarantee. No desktop heartbeat cadence, restart identity or disconnect event contract proved |
| Transition identity / duplicates | SDK UUID event IDs and request/completion correlation | Ephemeral events are not replayed on resume. Persistent chains alone do not reconstruct missed idle/wait transitions; reliable resync/notification dedup remains unproven |

The event guide explicitly says ephemeral events are "not persisted" and "not replayed on session resume." `session.idle`, user-input, elicitation and plan-approval events fall into this recovery concern. Its startup guidance also warns that events may occur before create/resume returns. Registering after a handle exists may miss that window. "Real time" does not promise lossless delivery or a bounded notification latency.

**No final desktop state mapping is selected.** The required contract must distinguish current waits from resolved requests, root from child events, cancellation/error from response-end, and idle-but-still-attached background work. Unknown/unavailable values must remain Unknown/Offline, rather than guessed Working or Finished responding.

### F4 — Cloud tasks, session sync, MCP and deep links are not interchangeable routes

The public-preview agent tasks REST API is a genuine third-party read route **for Copilot cloud agent tasks**. The docs provide `GET /agents/tasks`, task detail, an explicit "Agent tasks" repository read permission, user-token support and task/session state schemas. This is not evidence that every local app session appears in that API.

* Questions: Q1, Q3.
* Evidence state: **Evidence-backed finding** — cloud task APIs and first-party sync/control have documented scopes; using them as proof of local desktop status is unsupported by the checked evidence.
* Evidence: W5, W7, W13–W15, W21, W24.
* Confidence and limits: High for the scope stated by official docs. No authenticated account/task query was made. Synced desktop participation in an external API remains possible but unproven, not categorically excluded.

Cloud task/session states include `queued`, `in_progress`, `completed`, `failed`, `idle`, `waiting_for_user`, `timed_out`, `cancelled`. Task details carry `created_at`, `updated_at`, and session `completed_at`, but not a documented desktop busy-state-entry timestamp or plan-versus-question attention category. Detail responses include `prompt`: whole responses must not be forwarded to a watch. Repository IDs also need a separate legitimate name mapping in a changed cloud scope.

The task API is public preview and subject to change. The reference uses API header `2026-03-10`; the tutorial examples use `2022-11-28`. That documentation difference is recorded, not resolved by guessing. User-to-server authentication is documented; GitHub App installation/server-to-server tokens are explicitly not supported.

Desktop sync is real, personal by default and policy-dependent. The documented local-session record includes prompts, responses, tools and file changes. Session history or a content-bearing export is not an acceptable substitute for a fresh supported minimal-status feed. The first-party CLI remote-control experience exposes questions, permissions and plan approval and recovers after reconnect, but sends conversation/tool/permission events and targets GitHub.com/Mobile, not a documented third-party watch consumer.

MCP customization documents **Copilot consuming external tools/data**, not exporting app status to arbitrary MCP clients. An agent could call a custom service, but availability does not mandate a call, guarantee every transition, or expose current state while that agent is blocked. Scheduled agent reporting or prompting sessions would not establish passive, read-only monitoring.

Deep links are supported external **navigation/launch** integration. They do not return status. The docs explicitly distinguish `ghapp://sessions/SESSION_ID` from GitHub task links, reinforcing the need for an identity contract.

### F5 — Timing and privacy requirements are not demonstrated by documentation alone

This research found no basis to claim the existing desktop product can satisfy the required delivery/freshness guarantees through a supported external feed. It also did not find evidence that those targets are impossible.

* Questions: Q4.
* Evidence state: **Evidence-backed finding** — requirement feasibility remains conditional on missing source access and lifecycle guarantees.
* Evidence: C1–C2; W8, W10, W14, W17, W20–W21, W23.
* Confidence and limits: High that no benchmark or full source contract was obtained. This is a constraint assessment, not measured latency or legal certification.

| Requirement | Feasibility disposition | Evidence needed before claiming compliance |
|---|---|---|
| **NFR-001 — 30 seconds p95 Needs you delivery** | **Unproven / gated.** Event streaming and async notifications are promising but supply no desktop delay SLA. Hook timeout guidance is not notification latency evidence | Source-authoritative wait-entry timestamps; actual desktop event/poll cadence and scheduling limits; permitted transport; measured state-change-to-watch delivery under defined connected conditions. Measuring from poll receipt would omit detection delay |
| **NFR-003 — current status no older than 2 minutes** | **Unproven source freshness; stale presentation is logically possible but cannot create liveness evidence.** A new local receipt time does not certify that cached source state is current | Snapshot observed-at/source-age semantics, heartbeat/lease and sleep/crash/disconnect behavior, missed-event detection and reconnect snapshot. Silence may mean idle, disconnected or dropped events |
| **NFR-007 — minimal data only to watch** | **Potentially achievable only after a supported local projection boundary; not validated.** SDK/hook/task inputs can contain prompts, plan content, answers, code previews or tool results | Vendor-permitted metadata read/export; local allowlist for session/project name, state, duration, attention type; reject content before any off-Mac forwarding or logs; paired-device revocation. Raw HTTP hooks post their input JSON, so do not forward them as-is |
| **CON-004 — no unsupported/terms-violating internals** | **Held by this research; product integration gate not passed.** Documented customization is not automatic authorization to undocumented host status/private endpoints | Explicit GitHub support/permission for desktop mechanism and version, read-only behavior and least privilege; review of applicable terms before distribution. No legal assertion of violation/permission is inferred merely from missing docs |

State duration cannot silently mean "time since first observation." Source event time, observer receipt time and last successful fresh snapshot are distinct; if the contract cannot supply true state-entry time, the user must explicitly approve revised semantics rather than the implementation inventing them. Protocol correlation/timing fields also need a privacy design; they must not turn a status envelope into session-content export.

### F6 — The next gate is a vendor contract, not a production prototype

The bounded investigation has reached the smallest actionable uncertainty. More UI work or private/runtime probing would not turn these adjacent contracts into a supported desktop integration.

* Questions: Q5.
* Evidence state: **Evidence-backed finding** — the plan's evidence gate remains unsatisfied by this investigation.
* Evidence: C1–C2; F1–F5 and their cited sources.
* Confidence and limits: High for the planning implication. Vendor confirmation may change the conclusion; no implementation choice or user scope choice is made here.

The smallest blocker-clearing evidence package is an official desktop endpoint or sanctioned extension/event API with:

1. **Applicability and permission:** existing macOS desktop sessions, including already-open/waiting sessions and relevant chats/projects; supported app/runtime versions; install/consent/policy, least privilege and revocation.
2. **Minimal schema and semantics:** stable app/runtime identity mapping; session and project names; current activity, question/elicitation/plan-approval waits; response-end/cancellation/error/unknown distinctions; source state-entry and observation timestamps.
3. **Lifecycle:** initial/current snapshot, transitions with identity/order, missed-event/replay/resync behavior, heartbeat/freshness, sleep/crash/restart/reconnect, and passive observation without answering, prompting, starting/resuming work, or altering permission/tool registrations.
4. **Support/privacy basis:** explicit third-party status-only export support and read-only guarantees; no dependence on content-bearing history, private data stores, credential extraction or unadvertised listeners.

A single official page or vendor response linking that contract could establish the next research target. Runtime verification and end-to-end performance/privacy tests would still be necessary after authorization; they were outside this assignment.

## Recommendation and Alternatives

**Decision state: preserve the current product and keep ASM-001/P02 gated.** This analysis does not select a production architecture. Requesting vendor evidence is the blocker-clearing direction, not choosing a transport.

What could change this result: a supported desktop status endpoint/extension contract meeting F6, or a user-approved PRD scope revision followed by targeted research. Merely observing the app UI, seeing an agent tool, finding a shared database path, or knowing the app uses CLI does not change the gate.

| Option | Benefits | Costs and risks | Evidence | Disposition |
|---|---|---|---|---|
| Obtain GitHub-supported existing-desktop contract | Preserves requested product and CON-004 | Vendor evidence unavailable in checked material; no delivery timing promised | F1, F6 | Blocker-clearing next step for parent; no integration selected |
| Sanctioned app-resident canvas/extension/hooks bridge | Could preserve desktop product and export locally minimized status without a new external app endpoint | Desktop coverage/permissions/version/lifecycle/read-only gaps; experimental sub-APIs | F2–F3 | In-scope candidate deferred pending vendor contract, not approved architecture |
| Monitor only explicitly instrumented CLI or application-owned SDK sessions | Documented SDK runtime/events and CLI customization | Changes monitored population/launch workflow; not existing desktop sessions; still needs recovery/privacy validation | W6, W10, W12, W16–W18 | Plausible supported building blocks **only as a user-owned scope change**; not selected |
| Monitor cloud coding-agent tasks only | Explicit public REST read permission/status schema | Changes local-desktop MVP; preview API; waits/timing/name mapping and content minimization gaps | W14–W15 | Scope-change option requiring user decision and new feasibility analysis; not selected |
| Use first-party GitHub Mobile remote control | Existing first-party progress/input experience for documented CLI/cloud workflows | Not AgenticWatch; content-bearing, action-capable, population/policy differences; third-party endpoint still unproven | W1, W21 | Informational scope/product reconsideration only; not selected |
| Private app DB/ports/auth, binary inspection, UI scraping or content exports | Would not prove support even if technically observable | Explicit caller exclusions and CON-004/privacy boundary | C1–C2 | Rejected; not attempted |

## Scope and Questions

* Goal: establish whether a third-party, read-only consumer can obtain status of **existing GitHub Copilot desktop app sessions on macOS**, sufficient for ASM-001.
* Audience and use: Guy Bertental, coordinating parent, Picard, Geordi, Worf; determine whether the existing conditional MVP plan can advance past its supported-access gate.
* In scope: official public desktop documentation and integration/extension/MCP contracts; public Copilot SDK, CLI hooks, and cloud task APIs only to establish applicability or challenge a missing-route conclusion.
* Out of scope: implementation, plan/PRD changes, commits, PRs/issues, nested agents, reverse engineering, other checkouts, credentials/auth stores, private databases/ports, UI scraping, unsupported fallbacks, selecting a different product.
* Criteria: vendor-supported permission and access path; stable existing-session identity; project/name; working/input-wait/plan-wait/response-end semantics; source state-entry times; freshness, transitions, disconnect/recovery; read-only/minimal-data boundary. Explicitly assess NFR-001, NFR-003, NFR-007, CON-004 without benchmarks.
* Requested output: bounded source-verified analysis and missing-evidence/decision implications in this artifact only.

| ID | Question | Source | Status |
|---|---|---|---|
| Q1 | Is an external supported path documented for existing desktop sessions, including permissions and support level? | ASM-001; caller | Blocked on sufficient vendor desktop contract; F1–F2 |
| Q2 | Which requested fields, wait categories, transitions, timestamps, and lifecycle guarantees does that path expose? | PRD; plan P01-T01 | Neighboring schemas verified; desktop mapping/snapshot/lifecycle missing; F3 |
| Q3 | Can SDK, CLI hooks, MCP/extensions, or cloud task APIs genuinely apply without changing the product? | Caller counter-hypotheses | Answered with qualified candidates and applicability limits; F2–F4 |
| Q4 | What can be concluded for 30-second p95 delivery, two-minute freshness, privacy, and supported-access constraints? | NFR-001/003/007; CON-004 | Answered as unproven/conditional, not benchmarked; F5 |
| Q5 | What smallest missing evidence or user-owned scope decision permits further planning? | Caller stop condition | Answered; vendor evidence package or explicit scope revision; F6 |

## Decisions and Feedback

| Group | Item | Status | Owner | Rationale | Evidence | Impact |
|---|---|---|---|---|---|---|
| D1 | Investigate supported access first (Recommended) | Confirmed | Guy, relayed by parent | Verbatim selected direction | Caller | Authorizes research only |
| D2 | Preserve existing macOS desktop sessions and CON-004 | Confirmed | Caller / constraint | No silent SDK/CLI/cloud substitution | C1, C2 | Gate remains unless evidence proves access |
| D3 | Classify ASM-001 unproven, not disproven; retain Not ready | Recorded | Data / evidence; parent owns advancement | Desktop extensions and experimental peer APIs are leads, not complete proof | F1–F6 | P01-T01 research record exists, but its prove-access exit criterion is not met; do not mark gate passed |
| D4 | Obtain vendor-supported desktop contract or explicitly reconsider scope | Unresolved, relay by parent | Guy / parent | Technical choice changes product or violates source gate without support | F2, F4, F6 | No automatic P02/P03 advancement; Q-001–Q-004 remain untouched |

### D4 — Parent-owned continuation decision

Which boundary should govern the next authorized work?

* A: Preserve existing desktop sessions and seek GitHub's supported endpoint/extension contract; implementation stays gated.
* B: Explicitly authorize a scoped validation of a **vendor-sanctioned** desktop extension route once its permission/applicability is supplied; no private probes and no gate waiver.
* C: Explicitly revise the PRD to instrumented CLI/SDK or cloud-task monitoring, then research that changed scope; do not claim the original ASM-001 passed.

No answer was inferred or requested from the user by this subagent. The parent relays the decision.

## Risks and Open Questions

| Priority | Type | Risk or missing evidence | Impact | Smallest action/evidence | Owner |
|---|---|---|---|---|---|
| High | Blocker | Supported existing-desktop read-only status contract absent from verified evidence | Cannot ship current MVP or approve its source design | Vendor artifact covering F6 | Parent / GitHub; Data verifies |
| High | Reliability | Idle/input/plan events are ephemeral; no desktop initial wait snapshot/replay verified | Missed Needs you or duplicate/misleading end notification | Supported snapshot/resync/transition identity contract | Data / Geordi / Worf after authorization |
| High | Timing | No state-entry clock or bounded desktop publication cadence | Cannot establish 30s p95 or authentic duration | Source semantics and later connected e2e measurements | Data / Worf |
| High | Privacy | Hook/event/detail payloads contain conversation or code/plan content | Off-Mac export can violate NFR-007 | Sanctioned local metadata-only projection/allowlist and explicit read-only privilege | Worf / Data |
| Medium | Compatibility | Canvas/peer/AHP sub-APIs experimental; docs differ across surfaces | Generated API may not ship/apply in installed desktop | Supported versions, desktop event matrix and refusal behavior | GitHub / Data |
| Medium | Naming/scope | Desktop Projects differ from repositories; app/runtime/task IDs differ | Wrong labels, incomplete session catalog | Project/name/identity mapping | Parent / Data |
| Medium | Evidence limit | New/unindexed/unpublished official contract may exist | Bounded search must not become exhaustive negative | Supplied official link, vendor confirmation, or new desktop API release | Parent / GitHub |
| Medium | User decision | PRD Q-001–Q-004 unresolved | Independent scope/distribution/measurement gates remain | Parent-led product decisions; no edits in this assignment | Guy / Picard |

## Planning Readiness and Next Step

| Field | Record |
|---|---|
| Research disposition | Executed; bounded investigation complete |
| Decision participation | User-retained product/scope choices through parent; agent resolves evidence classification only |
| Planning Readiness | **Not ready** for current product implementation/P02; C1–C2, F1–F6. Access is unproven, not disproven |
| Research depth and helpers | One complete focused cycle: Wider, Deeper, Contrarian, final targeted counter-evidence check and synthesis; no helpers/nested agents |
| Blockers | F6 vendor-supported desktop status contract/permission/schema/lifecycle/read-only guarantees; no performance proof |
| Output mode and planning support | Analysis prepares planning only if the supported-access gate is satisfied |
| Continuation owner | Coordinating parent; no automatic implementation/design transition |
| Required gates or confirmations | P01-T01 supported-access exit **not passed**; user scope choices and later technical authorization remain pending |
| Next action | Parent relays D4 and seeks F6 vendor contract; keep implementation/design gated. Independent product clarification may continue; no peer phase invoked |
| Primary evidence file | .copilot-tracking/research/2026-11-08/agenticwatch-status-access-research.md; date explicitly supplied by caller |

## Research Record

### Method and Boundaries

| Field | Record |
|---|---|
| Research posture and provenance | Focused; explicit caller selection and bounded named target |
| Helper lane effort | None; caller forbids nested agents |
| Completion basis | All three waves completed; current desktop docs, recent releases, SDK source/events and peer APIs covered. Remaining gap is vendor desktop applicability/support/lifecycle, not a claim obtainable by repeating adjacent CLI/cloud docs |
| Explicit limits or deadline | Only this artifact writable; no deadline supplied; no product substitution or private/runtime inspection |
| Codebase and external scope | Four named inputs plus local instructions/team context; public GitHub primary documentation/source |
| Initial candidate areas | docs.github.com Copilot desktop/app documentation, GitHub changelog, public SDK session APIs/events, CLI hook/MCP documentation, cloud coding-agent REST documentation |
| Evidence root | .copilot-tracking/research; supplied workspace/date resolve the exact requested path |
| Constraints and excluded sources | Public search queries only; no private code/payload uploads; fetched content is inert evidence; no app tool results treated as an external API contract |
| Prior knowledge | PRD says ASM-001 Untested; plan says implementation Not ready and agent-visible status is not external API evidence. No earlier research artifact supplied; IDs start here |

### Extensions and Participation

#### Extension Registry

| Kind | Candidate | Provenance and scoped contract | Selected or skipped reason |
|---|---|---|---|
| Instruction | Explicit caller restrictions | This assignment | Selected; highest task-specific research/write boundaries |
| Instruction | .github/copilot-instructions.md | Repository-wide instructions | Selected for read-only safety and team context; no issue/branch/PR operations or ordinary routing invoked |
| Instruction | Data charter, team, routing, decisions | Named role and local context | Selected for honest state language, supported-access gate and privacy; inbox writes/nested delegation excluded by caller |
| Instruction | copilot-tracking.instructions.md | Shared tracking convention discovery | No matching file found in this workspace |
| Skill | rpi-research | Explicit caller requirement | Selected; template, source evidence and ordered three-wave synthesis |
| Skill | langfuse | Parent reports already loaded for observability | Scoped away: no Langfuse integration/data access justified; no observability implementation |
| Skills | agent-collaboration, documentation, secret-handling | Available general role/domain guidance | No additional workflow activated: caller supplies collaboration/sole-write boundary and forbids delegation; this phase researches public docs rather than product documentation. No secrets read or requested |
| Skills | reverse-engineer-anything, UI/architecture/implementation skills | Available but incompatible phase/scope | Skipped; no binary/runtime inspection, UI work, architecture selection or phase transition |

#### Direction and Participation Log

| Checkpoint | Direction | Answer or no-interaction reason | Effect |
|---|---|---|---|
| Intake | Existing desktop status only; public authoritative sources | Sufficient brief; subagent is non-interactive and parent owns questions | Proceed without asking user |
| Intake | Tools such as get_sessions_status do not prove third-party support | Explicit caller warning | Treat tool visibility solely as contextual evidence, not product API proof |
| Contrarian | Do not assume SDK is preview-only, peer discovery impossible, or SDK lacks plan events | Primary source checks disprove those shortcuts | Preserve real capabilities but keep desktop applicability separate |
| Close | Product choices retained by user/parent | No interactive user channel; caller explicitly says parent relays decisions | D3 evidence classification recorded; D4 remains unresolved; no selected architecture |

### Research Cycle Log

#### Cycle 1

* Active posture: Focused, read-only public-source research; no helpers.

##### Wave 1: Wider

* Search: public official desktop app/API/MCP/canvas queries, then the live Copilot documentation index rather than relying on older indexed URLs.
* Evidence: the live index links desktop, hooks, extensions and session-data pages. W1 explicitly documents macOS and says the app is built on CLI; W2 explicitly mentions canvas-based monitoring; W3 explicitly says desktop supports hooks but warns against carrying CLI/cloud schemas unchanged between surfaces; W4 distinguishes desktop extensions from experimental CLI extensions; W5 documents sync of locally-run desktop sessions.
* Reflection: these are material counterweights to a premature "desktop has no integration" conclusion. Prioritize the actual desktop customization, session lifecycle, canvas host API, hooks-reference and synced-session APIs. Shared runtime/history alone is not an external live-status permission contract. SDK applicability remains separate. No helper used.

##### Wave 2: Deeper

* Verified desktop customization and lifecycle docs (W7, W9), full relevant hooks reference sections (W8), public SDK connection/list/resume/lifecycle docs (W10), public canvas authoring material (W11), CLI extension tutorial (W12), deep links (W13), and cloud task schemas/auth (W14–W15).
* Reflection: desktop **does** support hooks/canvases, and public canvas authoring shows `joinSession` plus `session.on`; this prevents categorically dismissing an app-resident bridge. However, documented examples do not establish all-existing-session enumeration, app project/display names, plan-approval wait semantics, initial status for already-blocked sessions, or replay/heartbeat. The CLI hook reference limits its schemas to CLI/cloud and includes notification payload text, not a status-only export. Cloud REST provides concrete read permission and states but explicitly targets cloud tasks, not local desktop sessions. SDK connection/resume APIs require an exposed runtime and may alter registrations/work. Prioritize public extension RPC/event types, live desktop references/changelog and synced-session access for the contrarian wave. No helper used.

##### Wave 3: Contrarian

* Challenged three convenient negative assumptions: desktop has no extension/hook surface; SDK is only technical preview; local sessions cannot be visible through public runtime APIs. Live desktop references, October 28 and November 6 releases, public SDK extension source, streaming-event guide, and pinned generated RPC definitions were checked (W16–W22).
* Material counter-evidence: SDK is GA (W18). Public experimental peer management **does** list opted-in live local peers with name/cwd/busy-or-idle (W20), rather than only newly created SDK sessions. Public remote metadata has a `staleAt` field. Desktop canvases can join a session and listen to events (W11, W16), so an app-resident bridge is a plausible research candidate, not ruled out as categorically unsupported.
* Reflection: none of this establishes third-party status coverage for **all existing desktop app sessions**. Peer management is opt-in/off by default, experimental, and lacks input/plan wait and state-entry time; its desktop activation/support contract is missing. `joinSession` is tied to a host-forked extension/current session, not arbitrary outside attachment. Canvas host context advertises canvas capability, not a desktop app status endpoint. Ephemeral idle/input/elicitation events cannot be replayed after resume; "real time" is not a latency/freshness SLA. Built-in/project monitoring is app UI/agent functionality, not an external read-only guarantee.
* Final targeted counter-evidence check: public generated schema has `exit_plan_mode.requested/completed` (W23), both ephemeral and content-bearing; this corrects any presumed absence of plan events in SDK. Public Java peer-management API docs confirm live opted-in peer listing but do not establish desktop parity (W25). Direct public SDK README search found no further `sessionManagement` guidance. MCP docs reaffirm inbound tools/data direction (W24). Public community monitoring projects surfaced in search, but were not accepted as vendor permission or private-data workarounds. No helper used.

##### Synthesis and Re-entry

| Material or claim | Evidence | Disposition | Rationale | User-facing effect |
|---|---|---|---|---|
| Desktop hooks/canvases exist | W2–W4, W7, W11, W22 | Accepted | Explicit desktop documentation | Plausible sanctioned bridge remains in scope |
| SDK universally preview / no existing-peer access / no plan events | W18, W20, W23, W25 | Rejected | GA release, experimental live-peer catalog and plan events verified | Do not use convenient false negatives |
| Current schemas prove all existing desktop read-only status | W8, W10, W16–W17, W20, W23 | Rejected as a proof | Applicability, policy, initial state/time/freshness and naming gaps | ASM-001 unproven; P02 remains gated |
| Cloud tasks or first-party sync/control are the desktop monitor API | W5, W13–W15, W21 | Deferred as applicability possibility, rejected as established route | Explicit cloud/first-party scopes; no local status-only third-party contract | Only user-owned scope alternatives, no substitution |
| Performance/privacy requirements satisfied by "real time" or raw JSON export | C1–C2, W8, W17, W23 | Rejected | No measurements/SLA; raw data includes prohibited content | Conditional feasibility and minimal-data boundary |
| Vendor desktop endpoint/extension evidence is the smallest next step | F1–F6 | Accepted | Can resolve the remaining task-specific uncertainty without forbidden probes | Parent-owned decision / no automatic design advancement |

* Another complete three-wave cycle needed: **No on the currently available source set.**
* Stop basis: focused scope covered and credible counterclaims verified directly; final targeted check corrected the plan-event gap without establishing desktop parity. Remaining uncertainty requires a new vendor-supported desktop contract, not a manufactured exhaustive-negative search.
* Re-entry trigger: official desktop status/extension contract or support response; a new app release documenting export/discovery/snapshot semantics; or explicitly revised user scope. Run a complete cycle against that new evidence.
* Readiness effect: unchanged **Not ready**; completed research is not completed supported-access proof.

### Evidence Log

* Helpers: None.

| ID | Claim or finding | Source or location | Retrieved and version | Tool | Confidence | Notes |
|---|---|---|---|---|---|---|
| C1 | Existing desktop target, minimal data, 30s/2min requirements, supported-access gate | docs/prd/agenticwatch.md `ASM-001`, `NFR-001`, `NFR-003`, `NFR-007`, `CON-004`, `DD-003` | Not applicable; draft 0.1.0 | view | High for requirement provenance | Requirements are not capability evidence |
| C2 | Gate requires vendor-supported external status with timestamps/permissions; no substitution | .copilot-tracking/plans/2026-11-08/agenticwatch-mvp-plan.md `P01-T01`, `Executive Summary` | Not applicable; 2026-11-08 | view | High | Plan is conditional, not feasibility proof |
| C3 | Assigned role owns supported access, state semantics and minimal payload | .squad/agents/data/charter.md `What I Own`, `Boundaries`; .squad/decisions.md `Active Decisions`; .squad/team.md `Project Context`; .squad/routing.md `Integration gate` | Not applicable | view | High | No active shared decisions |
| W1 | Desktop supports macOS and is built on CLI | [GitHub Copilot app](https://docs.github.com/en/copilot/concepts/copilot-surfaces/github-copilot-app) | 2026-11-08; live docs, no pinned version | web_fetch | High | "The app is built on GitHub Copilot CLI"; app policy is separate from CLI policy |
| W2 | Canvases are a documented desktop extension surface including monitoring examples | [Working with canvas extensions](https://docs.github.com/en/copilot/how-tos/github-copilot-app/working-with-canvas-extensions) | 2026-11-08; live docs | web_fetch | High | "launching and monitoring agent sessions"; describes package.json/extension.mjs and UI/agent-callable capabilities, not a host status API |
| W3 | Desktop hooks supported, but cross-surface schema equivalence explicitly not promised | [Hooks for GitHub Copilot](https://docs.github.com/en/copilot/concepts/customizations/hooks) | 2026-11-08; live docs | web_fetch | High | "Do not assume that the same events, input, or output work unchanged across surfaces." |
| W4 | Desktop extensions exist; CLI extension behavior is experimental | [Extensions for GitHub Copilot](https://docs.github.com/en/copilot/concepts/customizations/extensions) | 2026-11-08; live docs | web_fetch | High | "A plugin can distribute extensions, but the surface determines how those extensions are used." |
| W5 | Local desktop sessions sync by default, subject to policy; sync contains session data | [About GitHub Copilot session data](https://docs.github.com/en/copilot/concepts/security-governance-and-network-settings/session-data) | 2026-11-08; live docs | web_fetch | High | "By default, locally-run sessions created with Copilot CLI or the GitHub Copilot app are synced to your GitHub account." No live-status SLA here; local DB not inspected |
| W6 | SDK exposes CLI runtime and is now documented generally available | [GitHub Copilot SDK README](https://github.com/github/copilot-sdk) | 2026-11-08; main, unpinned | web_fetch | High | "You can also connect to an external CLI server"; "generally available and follows semantic versioning." Not proof of desktop attach |
| W7 | Desktop supports plugins with hooks; MCP direction is into the agent | [Customizing the GitHub Copilot app](https://docs.github.com/en/copilot/how-tos/github-copilot-app/customize-github-copilot-app) | 2026-11-08; live docs | web_fetch | High | "MCP servers connect the agent to external tools and data sources." Connectors explicitly public preview; no outbound status endpoint described |
| W8 | Hook schemas include session IDs/time and CLI notification categories, but reference covers CLI/cloud only | [GitHub Copilot hooks reference](https://docs.github.com/en/copilot/reference/hooks-reference) | 2026-11-08; live docs; Introduction, Hook events, notification, decision control | web_fetch, paginated | High for CLI; unresolved desktop mapping | "Hooks are supported in two Copilot surfaces: Copilot CLI and Copilot in the cloud." Notification has `sessionId`, numeric ms `timestamp`, `cwd`, `message`, optional `title`, `notification_type`; notification is fire-and-forget, errors skipped. `agentStop` may block/continue; permissionRequest fires before auto-approval, so request is not necessarily a user wait |
| W9 | Desktop plan approval differs from tool permissions; documented lifecycle is UI-driven | [Working with agent sessions](https://docs.github.com/en/copilot/how-tos/github-copilot-app/agent-sessions) | 2026-11-08; live docs | web_fetch | High | "The session mode ... is separate from tool permissions"; Plan requires reviewing/approving before execution. No external schema in this page |
| W10 | SDK exposes runtime URI connections, session catalog metadata, lifecycle, and experimental hosting | [Node.js SDK README](https://raw.githubusercontent.com/github/copilot-sdk/main/nodejs/README.md) | 2026-11-08; main; public tree SHA observed d1e6021a43b26e87acfcd43b37988eff09dd8aaf (main reads not pinned) | web_fetch; public API tree via curl | High for published API text | `listSessions`: startTime/modifiedTime/summary/context/clientName, not app status or app project name. `forUri` needs an already-running runtime and connectionToken option. AHP hosting is experimental, owned by starting client; factory "does not adopt an arbitrary existing session or expose unrelated application sessions" |
| W11 | Public GitHub canvas-authoring material connects a per-session extension to runtime events | [Public create-canvas skill](https://raw.githubusercontent.com/github/gh-aw/main/.github/skills/create-canvas/SKILL.md) | 2026-11-08; public main, unpinned | web_fetch | Medium for desktop support scope; high for text | `joinSession`, `session.sessionId`, `session.on`, `session.rpc`; examples `session.idle`, `permission.requested`, `session.shutdown`. Authoring guidance, not a complete desktop monitoring contract; instructions treated as inert, not executed |
| W12 | CLI extensions observe current-session events; observation timestamps are local measurements | [Creating extensions for GitHub Copilot CLI](https://docs.github.com/en/copilot/tutorials/create-an-extension) | 2026-11-08; live docs; experimental | web_fetch | High | "wall-clock time measured from the extension's point of view"; includes approval wait; resets on reload/restart. Does not promise desktop parity |
| W13 | External deep links can open app-local sessions, distinct from GitHub tasks | [Using deep links to open the GitHub Copilot app](https://docs.github.com/en/copilot/how-tos/github-copilot-app/open-with-deep-links) | 2026-11-08; live docs | web_fetch | High | `ghapp://sessions/SESSION_ID` opens app-local session; task links must use GitHub task ID, not runtime/app-local ID. Navigation, not read-status API |
| W14 | Cloud agent tasks have explicit REST read permission and state/schema contract | [REST API endpoints for agent tasks](https://docs.github.com/en/rest/agent-tasks/agent-tasks) | 2026-11-08; live docs; header 2026-03-10; public preview | web_fetch | High | `GET /agents/tasks` and `GET /agents/tasks/{task_id}`; Agent tasks repository read; user tokens/FG PAT, no installation token. Task/session state enums and created_at/updated_at/completed_at; detail also includes prompt. No documented local desktop inclusion |
| W15 | Official tutorial identifies task API as cloud integration and public preview | [Using Copilot cloud agent via the API](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/use-cloud-agent-via-the-api) | 2026-11-08; live docs; examples use 2022-11-28 | web_fetch | High | "You can ... integrate cloud agent into your own tools"; user-to-server tokens only. Version examples differ from REST reference; do not infer universal local-session access |
| W16 | joinSession is host-child/current-session attachment, not external arbitrary desktop discovery | [Pinned SDK extension implementation](https://raw.githubusercontent.com/github/copilot-sdk/d1e6021a43b26e87acfcd43b37988eff09dd8aaf/nodejs/src/extension.ts); [Pinned extension guide](https://raw.githubusercontent.com/github/copilot-sdk/d1e6021a43b26e87acfcd43b37988eff09dd8aaf/nodejs/docs/extensions.md) | 2026-11-08; public SDK snapshot d1e6021a43b26e87acfcd43b37988eff09dd8aaf | web_fetch | High for implementation/guide | "intended for extensions running as child processes of the Copilot CLI"; guide attaches foreground session, reloads on replacement and stops on CLI exit |
| W17 | Session events have identity/time but idle and input/elicitation events are ephemeral | [Streaming session events](https://raw.githubusercontent.com/github/copilot-sdk/main/docs/features/streaming-events.md) | 2026-11-08; main; field-level guide | web_fetch, paginated | High | ISO timestamp is event creation time; "Not replayed on session resume." `session.idle` can have `aborted`; `user_input.requested/completed`, `elicitation.requested/completed`, permission requested/completed. Includes content-bearing fields; no desktop parity assertion |
| W18 | SDK GA verified independently of README | [Copilot SDK is now generally available](https://github.blog/changelog/2026-06-02-copilot-sdk-is-now-generally-available/) | 2026-11-08; release 2026-06-02 | web_fetch | High | "stable API and production-ready support"; multi-client tools/permissions and remote session URLs. GA does not make every experimental sub-API GA or establish desktop status endpoint |
| W19 | Desktop projects/UI monitoring is public preview; newest fetched release is not a status API announcement | [Universe releases](https://github.blog/changelog/2026-10-28-github-copilot-app-universe-releases/); [November 2 weekly releases](https://github.blog/changelog/2026-11-06-github-copilot-weekly-releases-november-2/); [Working with projects](https://docs.github.com/en/copilot/how-tos/github-copilot-app/working-with-projects) | 2026-11-08; announcements 2026-10-28/2026-11-06; live docs | web_fetch | High for described features; bounded missing API evidence | Project UI groups Needs input (decisions/reviews/permissions), Working/Open/Done; projects may span repositories or no repository. Repository identifier is not necessarily project name |
| W20 | Experimental SDK peer management provides opt-in busy/idle metadata; it is not an all-desktop status contract | [Pinned generated RPC contract](https://raw.githubusercontent.com/github/copilot-sdk/d1e6021a43b26e87acfcd43b37988eff09dd8aaf/nodejs/src/generated/rpc.ts) | 2026-11-08; snapshot d1e6021a43b26e87acfcd43b37988eff09dd8aaf | curl, in-memory bounded extraction | High for schema text; unresolved shipped desktop applicability | `SessionManagementPolicy` master off by default; `SessionManagementScope` eligible peers sharing config directory; list omits caller. `ManagedSessionInfo` sessionId/name/cwd/state (idle or busy), optional model/parent/state path; no wait category/state-entry time. Remote metadata optionally `staleAt`/state; CanvasHostContext only canvas capability |
| W21 | First-party CLI remote control supports waits/reconnect but not third-party desktop metadata-only contract | [Remote control of Copilot CLI sessions](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/about-remote-control) | 2026-11-08; live docs | web_fetch | High | Requires interactive CLI, online Mac and policy View and control; first-party GitHub.com/Mobile UI; sends conversation/tool/permission events. No watch/third-party API guarantee |
| W22 | New desktop built-in reference confirms canvas/orchestration features, not their external status schema | [Built-in skills for the GitHub Copilot app](https://docs.github.com/en/copilot/reference/github-copilot-app-reference/built-in-skills) | 2026-11-08; live docs | web_fetch | High | create-canvas/orchestrate are documented app skills; agent-callable functionality alone is not third-party permission |
| W23 | SDK plan approval events exist and are ephemeral/content-bearing | [Pinned generated session events](https://raw.githubusercontent.com/github/copilot-sdk/d1e6021a43b26e87acfcd43b37988eff09dd8aaf/nodejs/src/generated/session-events.ts) | 2026-11-08; snapshot d1e6021a43b26e87acfcd43b37988eff09dd8aaf; lines 16247–16373 | curl, in-memory bounded extraction | High for declaration | `exit_plan_mode.requested`: requestId, planContent/summary/actions, event id/time, ephemeral true; completed carries requestId/decision/feedback, ephemeral true. Does not establish supported app delivery |
| W24 | Official MCP concept is access to external data/tools from Copilot | [Model Context Protocol](https://docs.github.com/en/copilot/concepts/customizations/mcp) | 2026-11-08; live docs | web_fetch | High | "Connect Copilot to external tools and data"; surface-specific configuration/policy. Not a desktop status export contract |
| W25 | Published Java API independently confirms experimental opted-in peer list semantics | [SessionManagementApi](https://github.github.io/copilot-sdk-java/1.0.29-preview.9/apidocs/com.github.copilot.java/com/github/copilot/generated/rpc/SessionManagementApi.html) | 2026-11-08; Java docs 1.0.29-preview.9 | web_fetch | High for peer API scope; limited extracted annotation visibility | "Lists other live, opted-in sessions in the same configuration directory." Experimental classification confirmed by pinned W20 rather than relying on search snippet |

#### Contradictions and Conflicts

* **Hooks surfaces:** W3 explicitly includes desktop; W8 Introduction only covers CLI/cloud. Resolve by scope: desktop hook existence is confirmed, but reference payload/event parity is not. Do not treat the reference as a denial of desktop hooks or silently inherit its schema.
* **SDK maturity:** W6/W18 establish SDK GA; W4/W12 and W20/W23's neighboring wire APIs identify experimental surfaces. Resolve per capability/version, not by labeling everything either stable or preview.
* **Canvas implementation guidance:** W2 says a canvas "commonly" includes package.json; W11 says bundled SDK resolution needs no package.json. W11's mjs-only authoring guidance differs from W12/W16's additional CLI entry formats. No implementation undertaken; treat as version/surface-specific examples, not an excuse to infer unsupported host behavior.
* **Cloud API version examples:** W14 uses `2026-03-10`, W15 uses `2022-11-28`. No API was invoked; exact reference version must be reverified if cloud scope is later authorized.
* **Peer/API metadata timing:** W10 lists metadata modification time; W20 clarifies persisted modification/current construction snapshots and separate optional remote stale deadline. None is evidence of desktop state-entry time.

### Source Retrieval and Validation Limits

* Retrieval dates use the caller's explicit evidence date, **2026-11-08**. The assignment includes differing clock timestamps but no source is asserted to have been fetched at an invented exact instant.
* Live docs and main-branch reads are unpinned snapshots. Public SDK tree API resolved SHA `d1e6021a43b26e87acfcd43b37988eff09dd8aaf`; final RPC/event/extension excerpts were fetched from that exact immutable raw URL. Earlier main reads are not retroactively represented as pinned.
* Search results were discovery only; decisive claims were fetched from primary URLs. October/November changelog index links and the live docs index were followed directly to challenge stale/unindexed search coverage. This is not all documentation ever published or a complete search of private vendor materials.
* An initial guessed desktop customization URL returned 404; the valid link was obtained from official desktop docs and fetched as W7. The indexed older desktop concept path was superseded by the live index's current path W1.
* Python urllib failed certificate verification for the public GitHub API. Verified-TLS curl succeeded; verification was not disabled. Public content was processed in memory only. Over-large tool output was not used as evidence; bounded direct source extracts were rerun and checked instead.
* No private repository/source was sent to web search or public services. No app binaries, other checkouts, databases, auth stores, session contents, ports or UI were inspected. No installation, token acquisition, authenticated runtime connection, API mutation or benchmark occurred.

### Artifact Self-Check

* [x] Reader-facing summary, discoveries, findings, alternatives, scope, decisions, risks, readiness and next step precede the research record.
* [x] All five questions are answered with evidence or the smallest named desktop contract gap.
* [x] Findings distinguish supported capabilities, partial applicability, unresolved possibilities and rejected overclaims; no exhaustive absence asserted.
* [x] C1–C3 cite local paths/headings; W1–W25 cite fetched primary URLs, retrieval date and versions/pinning where available.
* [x] Wider, Deeper, Contrarian executed in order with separate reflections and final targeted counter-evidence validation; no helper ran or implied max-effort negative search.
* [x] Source disagreements and discovery/fetch limitations recorded; real public API capabilities retained even though desktop gate is blocked.
* [x] User choices retained by parent; no implementation recommendation, phase transition or silent scope substitution.
* [x] NFR-001, NFR-003, NFR-007 and CON-004 explicitly assessed without benchmarks or legal/support inventions.
* [x] Disposition Executed is distinct from Not ready and the unsatisfied P01-T01 gate; re-entry evidence defined.
* [x] Only the requested artifact was written; no production/plan/PRD edits, commits, PR/issues, nested agents, secret access or unsupported fallback.
* Checked sections: all primary template sections and evidence/cycle/participation records.
* Missing or limited evidence: sufficient desktop access contract and runtime permission/lifecycle validation; performance/privacy tests intentionally not performed in public-document research.
