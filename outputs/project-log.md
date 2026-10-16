# Movie booking system — project and learning log

Started: 16 October 2026 (Asia/Kolkata)

## Current position

Phase 0: Requirements and capacity estimation — started; requirements discussion pending.
No architecture, schema, API contract, or application code has been finalized.
No phase checkpoint has been attempted or passed.

## Purpose and context

Build a focused BookMyShow-style movie-ticket booking system to develop backend understanding and support clear, honest interview explanations.

The user wants to learn during the build, understand the stack and implementation logic, and explain every important decision without memorizing source code.

## Confirmed working agreements

- Discuss architecture and decisions before implementation.
- Initial audience: 1,000–2,000 total app users, not 1,000–2,000 simultaneous requests. Activity rates and peak traffic remain to be estimated.
- Study how the system could scale beyond this audience.
- Each phase ends with a checkpoint grill. Record answers, gaps, and follow-up; do not mark a checkpoint passed merely because an explanation was presented.
- Maintain this log throughout the project, including decisions, rejected alternatives, experiments, measured results, and learning checkpoints.
- Distinguish proposed assumptions, accepted requirements, and measured facts.
- Do not treat user-count targets as demonstrated capacity or simulated tests as real-user traction.

## Agreed roadmap

| Phase | Topic | Deliverables | Status |
| --- | --- | --- | --- |
| 0 | Requirements + capacity estimation | Feature list, scale numbers, what is out of scope | In progress |
| 1 | HLD | Component diagram, stack decisions | Not started |
| 2 | Data model | Schema, normalization, indexes | Not started |
| 3 | API design | Endpoints, request/response shapes, error codes | Not started |
| 4 | Core flow | Hold → confirm → expire, concurrency strategy | Not started |
| 5 | LLD | Layers, classes, patterns | Not started |
| 6 | Build | Code, including Phase 4's logic | Not started |
| 7 | Test | Concurrency test, load test, measured numbers | Not started |
| 8 | Scale | 10× / 100× bottleneck walkthrough | Not started |
| 9 | Interview pack | Two-minute pitch, HLD diagram, Q&A bank, résumé bullets | Not started |

Each phase includes a checkpoint grill. Earlier designs may be revised when later phases expose a problem; record the reason for revision.

## Proposals carried forward — not final decisions

- Candidate stack: React, Node.js, Express, PostgreSQL.
- Candidate main flow: choose a show, select seats, acquire a temporary hold, simulate payment, confirm booking.
- Start with seeded movies, theatres, and shows rather than a full administration interface.
- Suggested booking properties: one active allocation per show-seat; expiring holds; ownership checks; retry-safe confirmation; all-or-nothing multi-seat reservation.
- Distinguish a persisted temporary hold from a database transaction: a checkout timer should not require keeping a transaction open for its whole duration.
- Failure scenarios to examine: concurrent requests for the same seat, overlapping multi-seat requests, expired holds, abandoned checkout, repeated requests, and partial failures.
- Concurrency and load tests will be implemented and measured later. No traffic benchmark or correctness guarantee has yet been demonstrated.

## Phase 0 — open questions

1. Which customer and organizer actions belong in version one?
2. Simulated payment or a payment provider's test environment?
3. Are cancellation and refunds in scope?
4. Which geographic, theatre, screen, and show assumptions should we use?
5. What daily activity, peak concurrent activity, request rates, booking volume, and storage growth should we plan for?
6. Which correctness and performance targets define success?

## Decision log

| ID | Date | Decision | Reason / qualification |
| --- | --- | --- | --- |
| D001 | 2026-10-16 | Pursue a focused movie-ticket booking project | User selected this direction for backend learning and interview preparation. |
| D002 | 2026-10-16 | Plan for an initial audience of 1,000–2,000 total users | User explicitly distinguished audience size from simultaneous requests. |
| D003 | 2026-10-16 | Follow phases 0–9 with a checkpoint grill after each | User's chosen learning and delivery workflow. |

## Checkpoint record

No checkpoints attempted yet.

For each checkpoint, record the questions, the user's reasoning, corrections, unresolved gaps, and outcome.

## Experiments and measurements

None yet. Record setup, workload, environment, results, and limits when tests are run.

## Session history

### 16 October 2026 — roadmap recorded

- Captured the agreed ten-phase workflow and initial audience.
- Separated earlier recommendations from final design decisions.
- Opened Phase 0; feature boundaries and workload assumptions remain pending.
- Created this persistent project and learning log.

## Next step

Use https://github.com/Darth-hub/Event-Booking-System for project work. Agree on version-one features and exclusions, derive a plausible workload, and complete the Phase 0 checkpoint before moving to HLD.

### 16 October 2026 — repository setup requested

- User requested a GitHub repository and all subsequent project work to live in it, before starting the requirements discussion.
- Proposed repository name: `movie-booking-system`; private visibility selected as the initial default, not yet applied remotely.
- Prepared README and ignore rules alongside the existing roadmap and collaboration instructions.
- Initialized the local Git repository on `main` and saved the planning files in the initial commit after receiving access to the protected `.git` directory.
- GitHub CLI was not installed; a browser attempt timed out. GitHub integration was found but is not installed/connected; installation was suggested.
- Remote repository creation and initial push remain pending authenticated GitHub access. No GitHub URL has been created or verified.

### 16 October 2026 — GitHub connection verified

- User installed the GitHub integration; authenticated account verified as `Darth-hub`.
- The available integration supports repository file changes but exposes no repository-creation operation.
- Attempts to open the creation form through Chrome timed out; no remote repository creation was confirmed.
- Next prerequisite: create the empty private `movie-booking-system` repository under `Darth-hub`. Existing project files are ready for publication once the repository exists.

### 16 October 2026 — user-created repository verified

- User created `Darth-hub/Event-Booking-System` and explicitly noted that it is public; GitHub metadata confirmed the repository and public visibility.
- This name supersedes the earlier proposed repository name. The movie-show seat-booking scope remains unchanged.
- Initial repository documents comprise the README, ignore rules, collaboration instructions, and this canonical project log.
- No application code has been started; Phase 0 remains pending discussion and checkpoint.
