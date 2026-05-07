# Baseline Spec — Design Document

**Topic**: redisqueue baseline behavior spec
**Date**: 2026-05-07
**Branch**: main
**Anchor commit**: `db9f9f9` (constitution v1.0.0 ratified)
**Author**: brainstorming session (Claude Opus 4.7 + user)

This document captures the agreed-upon design for the **first** SpecKit spec in
the `redisqueue` fork. The spec it describes does **not** add behavior — it
freezes the library's current public contract and internal implementation as a
reference baseline so that future changes can be reasoned against it.

---

## 1. Goal

Produce a single SpecKit spec that documents the entire current behavior of
`redisqueue` at the chosen anchor commit (`db9f9f9`, 2026-05-07), enabling:

- Future PRs to cite specific clauses they preserve, change, or break.
- Reviewers to verify Constitution Principle II (Public API Stability) by
  diffing against this baseline.
- New maintainers to learn the library from one document instead of reading
  six `.go` files in arbitrary order.

This is **not** a redesign, refactor proposal, or feature spec.

## 2. Scope

### In scope

- Single SpecKit spec file at `specs/001-baseline/spec.md`.
- Coverage depth = **C** (per brainstorming session): public API + behavior
  guarantees + internal implementation snapshot.
- Source materials:
  - `consumer.go`, `producer.go`, `redis.go`, `message.go`, `signals.go`,
    `doc.go`
  - `consumer_test.go`, `producer_test.go`, `redis_test.go`,
    `signals_test.go`
  - `graphify-out/` — 14 communities, 88 nodes, 131 edges
- Anchor SHA: `db9f9f9`. Spec describes behavior **as of** this commit.

### Out of scope (explicitly excluded)

- CI migration (Travis → GitHub Actions) — separate spec.
- Upstream sync workflow — separate spec.
- Performance benchmarks — no benchmark suite currently exists.
- Any "how things should change" content — baseline is descriptive only.
- New features (DLQ, metrics, batch enqueue, etc.) — separate specs.
- CHANGELOG / release process — already governed by constitution.

## 3. Document structure

The spec follows `.specify/templates/spec-template.md` section names but
adapts content to a baseline-style document:

### 3.1 User Scenarios → "Library Use Cases"

Four use cases drawn from `README.md` and existing tests. Each carries a
priority and Given/When/Then acceptance scenarios that reference real test
functions:

| ID  | Priority | Use case |
|-----|----------|----------|
| US1 | P1 | Producer enqueues messages to a Redis Stream |
| US2 | P1 | Consumer processes messages with at-least-once delivery |
| US3 | P2 | Consumer survives panics, OS signals, and stuck handlers without losing work |
| US4 | P3 | One Consumer registered against multiple streams |

Acceptance scenarios cite test names (e.g., `TestEnqueue`, `TestRun`,
`TestNewSignalHandler`) rather than restating logic.

### 3.2 Functional Requirements → "Contract Clauses"

The bulk of the spec. Six clause groups, each clause:

- Carries a stable ID (`FR-Producer-001`, `FR-Delivery-003`, etc.).
- Cites source as `file.go:line` (e.g., `consumer.go:142`).
- Carries a **layer tag**:
  - `[Contract]` — promise to downstream users; cannot change without a
    MAJOR version bump (Constitution Principle II).
  - `[Snapshot]` — current implementation detail; may change in any release.

Groups:

| Prefix | Coverage |
|--------|----------|
| `FR-Producer-*`  | `NewProducer`, `NewProducerWithOptions`, `Enqueue`, `ProducerOptions` fields and defaults |
| `FR-Consumer-*`  | `NewConsumer`, `NewConsumerWithOptions`, `Register`, `RegisterWithLastID`, `Run`, shutdown semantics, `ConsumerOptions` |
| `FR-Delivery-*`  | At-least-once invariant, claim/ack flow, visibility timeout, reclaim loop semantics |
| `FR-Lifecycle-*` | Signal handling (SIGINT/SIGTERM), graceful shutdown, panic recovery, goroutine ownership |
| `FR-Errors-*`    | `Errors` channel surface, option validation errors, preflight check errors |
| `FR-Internal-*`  | `newRedisClient`, `redisPreflightChecks`, `incrementMessageID`, `registeredConsumer`, internal goroutine lifecycle |

The layer tag is what makes C-depth coverage safe: future refactors only need
to update `[Snapshot]` clauses; `[Contract]` clauses are protected by the
constitution.

### 3.3 Key Entities

Public: `Producer`, `Consumer`, `Message`, `ProducerOptions`,
`ConsumerOptions`, `ConsumerFunc`.

Internal (cited but explicitly snapshot-only): `registeredConsumer`,
`newRedisClient`, `redisPreflightChecks`, `incrementMessageID`,
`newSignalHandler`.

### 3.4 Success Criteria → "Verification"

- **SC-001**: All four `*_test.go` files pass against a real Redis instance.
- **SC-002**: `go test -race ./...` is clean.
- **SC-003**: `golangci-lint run` against in-repo `.golangci.yml` is clean.
- **SC-004**: Every `FR-*` clause maps to ≥ 1 existing test. Clauses with
  no test coverage are listed in the spec's **Gap Report** appendix.
- **SC-005**: `gofmt -s` produces no diff; `go vet ./...` is clean.

SC-004 is the most useful one — it forces the spec author to discover any
behavior the current test suite does NOT actually pin, which is itself
valuable output even though the baseline spec is supposed to be descriptive.

### 3.5 Assumptions

- The spec is a snapshot at `main@db9f9f9` / 2026-05-07.
- The spec describes "what is", not "what should be".
- `[Snapshot]` clauses age — Constitution governance requires PRs that
  change internal mechanics to update affected snapshot clauses in the same
  PR.
- The library's external dependency surface (`go-redis/v7`, Redis server
  semantics) is treated as a black box; we describe `redisqueue`'s use of
  it, not Redis itself.

### 3.6 Out of Scope

(See section 2 above; restated in the spec for self-containment.)

### 3.7 Gap Report (appendix)

Listed at end of spec. For each `FR-*` clause without a corresponding test,
record:

- Clause ID
- Why no test exists today (best-effort guess from reading tests)
- Risk level (High / Medium / Low) — high if `[Contract]` and untested

This appendix is the most actionable output for future maintenance work
(potential follow-up specs to close gaps).

## 4. Method to write the spec

1. Read all six `.go` files fully (no skim).
2. Read all four `*_test.go` files fully.
3. Use `/graphify query "..."` only to disambiguate when source reading is
   ambiguous; the source is the source of truth.
4. Draft `FR-*` clauses bottom-up from the code, tagging each as
   `[Contract]` or `[Snapshot]` based on whether downstream users observe it.
5. Build the traceability matrix `FR-* ↔ test function`.
6. Identify gaps; populate appendix.
7. Self-review (placeholders, contradictions, ambiguity, scope).

## 5. Estimated size

- Rough projection: 600–900 lines.
- Trigger for re-discussion: if draft exceeds 1200 lines, pause and
  re-evaluate whether to split (current decision: single file).

## 6. Layered tag rationale (recap)

The layer tag (`[Contract]` vs `[Snapshot]`) is the load-bearing design
choice. Without it, C-depth coverage means every internal refactor breaks
the spec, which kills its long-term value. With it:

- `[Contract]` clauses change rarely and only with major version bumps.
  These are the part of the baseline that future work cites against.
- `[Snapshot]` clauses age, and that's expected. They exist to help a
  reader understand "how does this work today" — useful context, not a
  promise.

This split is what lets the user's choice of "single comprehensive spec"
(structure A) coexist with C-depth coverage without locking the
implementation in concrete.

## 7. What this enables next

Once the baseline spec exists, follow-up specs become much easier to write:

- A feature spec lists which `FR-*` clauses it adds, modifies, or removes.
- A bug-fix spec cites the `FR-*` clause whose stated behavior was wrong.
- A refactor PR cites only `[Snapshot]` clauses it touches.
- An upstream sync spec lists `FR-*` clauses that diverge from upstream.

Reviewers can then enforce Constitution Principle II mechanically: if a PR
modifies a `[Contract]` clause without raising the major version, block it.

## 8. Risks

- **Risk**: The spec author (me, in the next session) misclassifies a
  `[Snapshot]` clause as `[Contract]` and locks the implementation.
  - **Mitigation**: Conservative default — when in doubt, mark `[Snapshot]`.
    `[Contract]` requires explicit user-facing observability (return type,
    documented behavior, error type, channel surface).
- **Risk**: The baseline ages silently as code drifts.
  - **Mitigation**: Constitution already requires PR description to call
    out which clauses are affected. SC-004's traceability matrix gets
    reviewed when the test suite changes.
- **Risk**: 600–900 lines is daunting to read.
  - **Mitigation**: TOC at top of spec, anchor links per FR group; gap
    report in its own appendix.

## 9. Approval & next step

Approved by user during 2026-05-07 brainstorming session.

**Next**: invoke `superpowers:writing-plans` to produce the implementation
plan that drives the actual writing of `specs/001-baseline/spec.md`.
