<!--
SYNC IMPACT REPORT
==================
Version change: (template / unfilled) → 1.0.0
Bump rationale: Initial ratification — replaces unfilled template placeholders
with concrete principles for the redisqueue Go library. MAJOR baseline because
no prior numbered version existed.

Modified principles:
- [PRINCIPLE_1_NAME] → I. Library-First & Single Responsibility
- [PRINCIPLE_2_NAME] → II. Public API Stability (Semantic Versioning, NON-NEGOTIABLE)
- [PRINCIPLE_3_NAME] → III. Test-First Discipline (NON-NEGOTIABLE)
- [PRINCIPLE_4_NAME] → IV. At-Least-Once Delivery Guarantees (NON-NEGOTIABLE)
- [PRINCIPLE_5_NAME] → V. Operational Robustness

Added sections:
- Tooling & Quality Gates (replaces [SECTION_2])
- Development Workflow & Review (replaces [SECTION_3])
- Governance

Removed sections: none (all template placeholders consumed)

Templates requiring updates:
- ✅ .specify/templates/plan-template.md — Constitution Check gate stays generic
  ("Gates determined based on constitution file"); aligns with these principles
  without textual edits.
- ✅ .specify/templates/spec-template.md — no constitution-coupled language;
  no edits required.
- ✅ .specify/templates/tasks-template.md — no constitution-coupled language;
  test-task wording remains optional and compatible with Principle III.
- ✅ .specify/templates/checklist-template.md — generic; no edits required.
- ⚠ .claude/commands/*.md — none observed referencing principle names; no edits
  required at this time.
- ✅ README.md — runtime guidance stays describing usage; no edits required.
- ✅ CLAUDE.md — agent guidance compatible; no edits required.

Follow-up TODOs: none. RATIFICATION_DATE recorded as the date this constitution
was first filled in (2026-05-07); upstream library inception (2019-07-13) is
documented in CHANGELOG/graphify-out and is intentionally not used here because
this constitution governs the fork's working agreement, not upstream history.
-->

# redisqueue Constitution

## Core Principles

### I. Library-First & Single Responsibility

`redisqueue` is and remains a focused Go library that provides a producer and a
consumer over Redis Streams. The package MUST stay self-contained, importable
without side effects, and free of organizational-only abstractions.

- New functionality MUST extend the producer/consumer model around Redis
  Streams; out-of-scope concerns (alternative brokers, message routing layers,
  application servers, CLI binaries beyond simple examples) are rejected.
- Public types and functions MUST have a clear, documented purpose. "Helper
  packages" that exist solely to group code are not allowed.
- Examples in `README.md` MUST remain runnable against a real Redis.

**Rationale**: The library's value comes from doing one thing well. Scope creep
turns a queue primitive into an unmaintainable framework.

### II. Public API Stability (Semantic Versioning, NON-NEGOTIABLE)

The module path encodes the major version (`github.com/robinjoseph08/redisqueue/v2`).
Backward-incompatible changes to exported identifiers MUST cause a major
version bump and a new module path; they MUST NOT be smuggled into a minor or
patch release.

- Exported names, struct field shapes, function signatures, and observable
  behavior of `Producer`, `Consumer`, `Message`, and option structs are part of
  the public contract.
- Adding new optional fields with safe zero-value defaults is MINOR.
- Bug fixes that do not change documented behavior are PATCH.
- Removing or renaming exported identifiers, changing required field semantics,
  or altering delivery guarantees is MAJOR and MUST ship on a new module path.
- `CHANGELOG.md` MUST record every release with the bump category.

**Rationale**: Downstream users depend on `go get` resolving without surprise
breakage. Semantic import versioning is the only mechanism Go provides to
honor that contract.

### III. Test-First Discipline (NON-NEGOTIABLE)

Every behavior change MUST be accompanied by tests written before or alongside
the implementation, and those tests MUST exercise the same code path the user
would.

- Bug fixes MUST include a regression test that fails before the fix and
  passes after.
- New public API MUST include unit tests for option validation and integration
  tests that drive a real Redis (matching the existing `consumer_test.go` /
  `producer_test.go` / `redis_test.go` style).
- Mocked Redis is NOT a substitute for real-Redis tests when the change touches
  stream commands, claim/ack flow, or reclaim logic.
- The race detector (`go test -race`) MUST pass for any change that touches
  goroutine-managing code (consumer scheduler, signal handler, error channel).

**Rationale**: The library's correctness story (at-least-once, visibility
timeout, graceful shutdown) only holds if tests reproduce the conditions where
those guarantees matter.

### IV. At-Least-Once Delivery Guarantees (NON-NEGOTIABLE)

The claim/ack contract is the library's load-bearing promise. Changes MUST
preserve at-least-once semantics under crash, panic, and signal scenarios.

- A message is acknowledged ONLY after the user-supplied `ConsumerFunc`
  returns `nil`. Acks before successful processing are forbidden.
- Visibility timeout, reclaim interval, and pending-entry list handling MUST
  ensure that messages stuck on a dead consumer are eventually re-delivered.
- Graceful shutdown (SIGINT/SIGTERM) MUST allow in-flight messages to finish
  or be re-claimable; it MUST NOT silently drop work.
- Panics inside a `ConsumerFunc` MUST be recovered, surfaced on the errors
  channel, and MUST NOT acknowledge the offending message.
- Any change that weakens these guarantees — even temporarily — REQUIRES a
  major version bump and an explicit migration note.

**Rationale**: Users adopt this library specifically because it survives
process death without losing work. Eroding that guarantee silently is the
worst possible failure mode.

### V. Operational Robustness

The library is consumed by long-running services. It MUST behave well in
production conditions without requiring the caller to write defensive
scaffolding around it.

- Errors that the consumer cannot handle internally MUST be delivered on the
  exposed errors channel; they MUST NOT be logged-and-swallowed or panic the
  process.
- Configuration knobs (`Concurrency`, `BufferSize`, `VisibilityTimeout`,
  `BlockingTimeout`, `ReclaimInterval`, `StreamMaxLength`) MUST validate at
  construction time; invalid combinations MUST fail fast with a descriptive
  error.
- The library MUST NOT spawn goroutines that outlive the `Consumer` or
  `Producer` instance that owns them.
- Logging is the caller's responsibility; the library MUST NOT introduce a
  hard dependency on a specific logger.

**Rationale**: A queue library that leaks goroutines, hides errors, or pulls in
an opinionated logger is unfit for the production services that depend on it.

## Tooling & Quality Gates

The following gates MUST pass on every change before merge:

- `gofmt -s` produces no diff on changed files.
- `go vet ./...` is clean.
- `golangci-lint run` is clean against the in-repo `.golangci.yml`.
- `go test ./...` and `go test -race ./...` pass against a real Redis instance
  (the `Makefile` and `scripts/` orchestrate this; CI is the source of truth).
- Code coverage MUST NOT drop materially on a change; coverage exists as a
  signal of test discipline (Principle III), not as a vanity metric.

Tooling versions are pinned via `.go-version` and `tools.go`. Bumping a tool
version is a normal change and follows the same review path as code.

## Development Workflow & Review

- All changes land via pull request. Direct pushes to `main` or `master` are
  forbidden; emergency exceptions require a follow-up PR documenting the
  bypass.
- Every PR MUST link to the user-visible motivation (issue, bug report, or
  spec under `specs/`) and state which principles it touches.
- A reviewer MUST verify that Principles II–IV are not silently violated.
  Reviewer sign-off implies they confirmed: API stability category is correct,
  tests are present and meaningful, and delivery semantics are intact.
- The `master` branch is preserved for upstream history compatibility (per
  `x_fork.branch-origin.md`); `main` is the working default. Rewriting either
  branch's history is forbidden.
- `CHANGELOG.md` MUST be updated in the same PR that introduces a
  user-visible change.

## Governance

This constitution supersedes ad-hoc conventions. When this document and any
other guidance disagree, this document wins until amended.

- **Amendments** are made by PR that modifies `.specify/memory/constitution.md`
  and runs `/speckit-constitution` so the Sync Impact Report at the top of this
  file is updated. Amendments MUST justify the version bump category and list
  affected templates.
- **Versioning policy** for this constitution mirrors Principle II:
  - MAJOR: a principle is removed or its rule materially weakened.
  - MINOR: a new principle/section is added, or a principle is materially
    expanded.
  - PATCH: clarifications, wording fixes, non-semantic refinements.
- **Compliance review**: any reviewer or maintainer MAY block a merge by
  citing a specific principle. The cited principle name and the violating
  behavior MUST appear in the review comment so the conversation is grounded.
- **Complexity must be justified**: deviations from "the simplest thing that
  satisfies the principles" require an entry in the PR description naming the
  forcing function. Speculative flexibility is not a forcing function.
- **Runtime guidance**: contributors and AI agents working in this repo follow
  `CLAUDE.md` and `README.md` for day-to-day workflow; those documents MUST
  defer to this constitution on conflicts.

**Version**: 1.0.0 | **Ratified**: 2026-05-07 | **Last Amended**: 2026-05-07
