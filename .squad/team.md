# Squad Team

> agenticwatch

## Coordinator

| Name | Role | Notes |
|------|------|-------|
| Squad | Coordinator | Routes work, enforces handoffs and reviewer gates. |

## Members

| Name | Role | Charter | Status |
|------|------|---------|--------|
| Picard | Lead & Architect | .squad/agents/picard/charter.md | 🏗️ Lead |
| Data | Copilot Integration Engineer | .squad/agents/data/charter.md | 🔧 Integration |
| Geordi | Apple Watch & iPhone Engineer | .squad/agents/geordi/charter.md | ⚙️ Apple platforms |
| Troi | UX & Accessibility Designer | .squad/agents/troi/charter.md | ⚛️ Design |
| Worf | Quality & Security Engineer | .squad/agents/worf/charter.md | 🧪 Quality |
| Scribe | Session Logger & Memory Manager | .squad/agents/scribe/charter.md | 📋 Silent |
| Ralph | Work Monitor | .squad/agents/ralph/charter.md | 🔄 Monitor |
| Rai | RAI Reviewer | .squad/agents/Rai/charter.md | 🛡️ RAI |
| Fact Checker | Verification & Devil's Advocate | .squad/agents/fact-checker/charter.md | 🔍 Verifier |


## Coding Agent

<!-- copilot-auto-assign: false -->

| Name | Role | Charter | Status |
|------|------|---------|--------|
| @copilot | Coding Agent | — | 🤖 Coding Agent |

### Capabilities

**🟢 Good fit — auto-route when enabled:**
- Bug fixes with clear reproduction steps
- Test coverage (adding missing tests, fixing flaky tests)
- Lint/format fixes and code style cleanup
- Dependency updates and version bumps
- Small isolated features with clear specs
- Boilerplate/scaffolding generation
- Documentation fixes and README updates

**🟡 Needs review — route to @copilot but flag for squad member PR review:**
- Medium features with clear specs and acceptance criteria
- Refactoring with existing test coverage
- API endpoint additions following established patterns
- Migration scripts with well-defined schemas

**🔴 Not suitable — route to squad member instead:**
- Architecture decisions and system design
- Multi-system integration requiring coordination
- Ambiguous requirements needing clarification
- Security-critical changes (auth, encryption, access control)
- Performance-critical paths requiring benchmarking
- Changes requiring cross-team discussion

## Project Context

- **Project:** agenticwatch
- **Created:** 2026-11-08
- **PRD:** [AgenticWatch](../docs/prd/agenticwatch.md) (PRD-AGENTICWATCH-001)
- **Theme:** Star Trek: The Next Generation. Names are persistent identifiers, not role-play.
- **Product:** Apple Watch-first monitoring of personal Copilot app sessions on macOS, with an iPhone companion.
- **MVP:** Session status, attention notifications, freshness indicators, and complications; no watch-side session actions.
- **Status language:** Working, Needs you, Finished responding, Unknown/Offline. Idle is not proof of task success.
- **Privacy:** Share only session name, project name, state, state duration, and attention type; never prompts, responses, code, or file contents.
- **Open dependency:** Validate a supported status integration (ASM-001) before implementation. Technical architecture, stack, supported OS versions, and PRD questions remain undecided.
