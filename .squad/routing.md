# Work Routing

How to decide who handles what.

## Routing Table

| Work Type | Route To | Examples |
|-----------|----------|----------|
| Architecture & technical design | Picard | Integration feasibility gate, component contracts, platform trade-offs |
| Copilot status integration | Data | Supported access spike (ASM-001), state mapping, freshness, minimal-data payload |
| Apple Watch & iPhone engineering | Geordi | Watch app, companion, haptics, complications, connection recovery |
| UX & accessibility | Troi | Attention-first views, honest labels, setup, VoiceOver, Dynamic Type |
| Security & privacy | Worf | Pairing/revocation, protected transport/storage, lock-screen disclosure |
| Code review | Picard, Worf | Architecture correctness, regressions, acceptance evidence |
| Testing | Worf | PRD acceptance criteria, duplicate alerts, latency, stale/offline recovery |
| Scope & priorities | Picard | Monitoring-only MVP, dependencies, unresolved PRD questions |
| Session logging | Scribe | Automatic — never needs routing |
| Work monitoring | Ralph | Scan backlog when explicitly activated; report blockers |
| RAI review | Rai | Content safety, bias checks, credential detection, ethical review |
| Claim verification | Fact Checker | Supported API evidence, platform limits, research claims, counter-hypotheses |

## Issue Routing

| Label | Action | Who |
|-------|--------|-----|
| `squad` | Triage: analyze issue, assign `squad:{member}` label | Picard |
| `squad:{name}` | Pick up issue and complete the work | Named member |

### How Issue Assignment Works

1. When a GitHub issue gets the `squad` label, **Picard** triages it — analyzing content, assigning the right `squad:{member}` label, and commenting with triage notes.
2. When a `squad:{member}` label is applied, that member picks up the issue in their next session.
3. Members can reassign by removing their label and adding another member's label.
4. The `squad` label is the "inbox" — untriaged issues waiting for Lead review.

## Rules

1. **Eager by default** — spawn all agents who could usefully start work, including anticipatory downstream work.
2. **Scribe always runs** after substantial work, always as `mode: "background"`. Never blocks.
3. **Quick facts → coordinator answers directly.** Don't spawn an agent for "what port does the server run on?"
4. **When two agents could handle it**, pick the one whose domain is the primary concern.
5. **"Team, ..." → fan-out.** Spawn all relevant agents in parallel as `mode: "background"`.
6. **Anticipate downstream work.** If a feature is being built, spawn the tester to write test cases from requirements simultaneously.
7. **Issue-labeled work** — when a `squad:{member}` label is applied to an issue, route to that member. Picard handles all `squad` (base label) triage.
8. **Integration gate** — Data and Picard validate ASM-001 before committing to product implementation; never use unsupported or terms-violating access.
9. **No scope expansion** — creating the team does not activate Ralph, assign issues, or approve implementation. Preserve unresolved PRD questions and disabled @copilot auto-assignment.
