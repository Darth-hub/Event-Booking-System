# Movie booking system — project and learning log

Started: 16 October 2026 (Asia/Kolkata)

## Current position

Phase 2: Data model — catalogue and transactional columns/constraints proposed. Normalization/composite-key review, remaining scheduling integrity, query indexes, and the phase checkpoint remain pending. See [data model notes](phase-2-data-model.md), [Phase 0 requirements](phase-0-requirements.md), and [HLD](phase-1-hld.md).
An initial HLD baseline is documented; schema, API contracts, and application implementation remain pending.
Phase 0 checkpoint attempted; a daily-versus-concurrent-user follow-up remains unassessed. The Phase 1 hold-durability/statelessness checkpoint passed.

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
| 0 | Requirements + capacity estimation | Feature list, scale numbers, what is out of scope | Discussed; checkpoint follow-up unassessed |
| 1 | HLD | Component diagram, stack decisions | Initial design completed; asked checkpoint passed |
| 2 | Data model | Schema, normalization, indexes | In progress |
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

### Phase 0 — first attempt

1. Registered users versus RPS: user correctly identified 2,000 as total registrations and gave `RPS = active users × requests per user per second`, but described active users as 20% of total. Correction: 20% was the assumed daily active proportion (400 DAU); the assumed peak simultaneously active population was 50. Checkpoint remains pending a short follow-up on which population belongs in the peak-RPS calculation.
2. Hold expiry: user correctly explained that unbooked seats must return to availability. Clarification supplied: explicit release may never arrive after a closed tab, lost connection, or crashed client; server-side expiry must work independently.
3. Lost confirmation response: user correctly stated that retry must recover the committed booking without creating another and that payment success must be distinguished from booking confirmation.

Outcome: core ideas mostly understood; follow-up unassessed. The user subsequently asked to move ahead, so HLD has started without recording a checkpoint pass. Reference answer supplied: 50 concurrent users / five seconds = 10 RPS.

For each checkpoint, record the questions, the user's reasoning, corrections, unresolved gaps, and outcome.

### Phase 1 checkpoint — API restart during a hold

User correctly explained that a committed hold lives in PostgreSQL rather than instance memory, and that another instance can retrieve it using its hold identity and authenticated ownership. Their shared-seat-chart analogy correctly captures why API restarts do not destroy durable holds.

Outcome: passed for the asked checkpoint. Clarified that deadlines still apply through restarts, temporary request memory is compatible with statelessness, and SELECT alone is not the final atomic confirmation algorithm. Illustrative schema fields remain provisional.

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
- Published all four initial documents through the GitHub integration and verified their contents against the local files.
- Configured local `origin` to the verified GitHub URL. Direct Git fetch timed out, so local and remote commit histories are not yet synchronized; use the integration for remote file updates until Git transport works, then reconcile histories without overwriting remote work.

### 16 October 2026 — workflow efficiency and scope discussion

- User requested concise exchanges, manual participation, and targeted connector use to conserve usage. Batch log updates at meaningful milestones; exact per-message token consumption is not available.
- User expanded version one with early hold release, hold limits, city/date filtering, authoritative server pricing, and a lost-response simulation.
- Adopted the invariant of at most one confirmed booking per show-seat; payment failures retain holds until the original expiry. Cancellation, refunds, WebSockets, refresh tokens, OAuth, and an admin interface are outside version one.
- Recorded a proposed five-minute hold duration and a ten-seat cap in `phase-0-requirements.md`; these defaults still need user confirmation.
- Captured limitations: browser-close release is best effort, per-account hold limits are incomplete abuse protection, and cancellation requires more than a new enum value.
- Scope and consistency requirements are documented; architecture, schema representation, capacity estimates, and checkpoint outcomes remain open.

### 16 October 2026 — capacity lesson, study notes, and first checkpoint

- User requested a concise “Notes to keep” block at each teaching step, including concepts, decisions, reasons, and interview takeaways.
- Planning assumptions presented, not measured or explicitly confirmed: 2,000 registered users, 400 DAU (20%), 20 API requests per daily user, 8,000 API requests/day, 50 peak concurrent active users, and one request per five seconds per active user (10 peak RPS). Static assets excluded.
- Booking assumptions presented: 10% of daily visitors make one booking, yielding 40 bookings/day; three seats per booking yields 120 booked seats/day, about 14,600 bookings and 43,800 booked-seat records per year. Storage bytes and retention policy remain undecided.
- Proposed performance target: p95 backend response time at most 500 ms under the planned 10 RPS workload, including database work and excluding network travel and deliberate payment-simulation delays. Environment, request mix, measurement window, and achieved capacity remain unspecified.
- Discussed correctness, atomic multi-seat acquisition, retry safety, ownership authorization, server pricing, and safe handling of uncertain mutation outcomes.
- Recorded the user's three Phase 0 checkpoint answers above. Daily versus concurrent activity needs clarification before HLD; no checkpoint pass recorded.

### 16 October 2026 — HLD started at user request

- User asked to move ahead without answering the checkpoint follow-up. Proceeded to HLD; no answer or pass inferred.
- Proposed starting architecture: React browser client → Node.js/Express backend → PostgreSQL. One deployable backend with internal responsibilities for authentication, browsing, holds, bookings, simulated payments, and expiry cleanup; these are not separate network services.
- Database is authoritative for seat ownership, hold deadlines, prices, bookings, and retry outcomes. Request paths must enforce expiry independently of cleanup punctuality.
- Teaching focus: component responsibilities, frontend/backend trust boundary, and why short database transactions support all-or-nothing booking changes. Transactions alone do not establish the no-double-booking guarantee; concurrency strategy and constraints remain for later phases.
- Added Notes to keep in the teaching response. Architecture and stack remain proposals for discussion; implementation has not started.

### 16 October 2026 — HLD detail and payment boundary revised

- User requested internal Auth, Catalog, Hold, Booking, and Payment boxes; a swappable mock gateway; a scheduled housekeeping job; database responsibilities; JWT; stateless API labeling; two-way links; and a hold-to-confirm sequence.
- Saved diagrams and study notes in `phase-1-hld.md`. Module boxes are code boundaries in one process. JWT was requested by the user; token handling details remain open.
- Corrected the proposed sequence: conditional multi-seat writes require all-or-nothing validation; idempotency needs atomic claiming; persisted PAYMENT_PENDING is distinct from a database lock; gateway calls occur outside database transactions.
- Distinguished a committed booking with a lost API response from an unknown gateway outcome. Both API and gateway operations need retry identities; their mapping must survive restarts.
- New unresolved requirement: uncertain payments need protected inventory and outcome recovery; blindly applying the ordinary hold expiry can sell seats while an earlier charge may have succeeded. Recovery policy, timing, and allocation-release rules are mandatory Phase 4 work.
- PostgreSQL remains the only application persistent store. Mock gateway replay/recovery behavior must be designed explicitly; exact API contracts, schema, and concurrency implementation remain provisional.

### 16 October 2026 — HLD checkpoint and data-model transition

- Recorded React/Node.js/Express/PostgreSQL rationale, operational trade-offs, and the difference between statelessness and high availability in the HLD document.
- User passed the asked HLD checkpoint on persisted holds and API restarts. No implementation or load-test result is implied.
- Starting Phase 2 with the distinction between a physical seat in a screen and inventory for that seat in a particular show. Booking/hold status belongs to the show-seat allocation, not the physical seat itself.
- Next work: agree on entities and relationships, then schema, normalization, constraints, and indexes. Detailed payment recovery and transaction algorithms remain later-phase work.

### 16 October 2026 — catalogue and booking relationships

- Explained cities → theatres → screens → physical seats, with movies → shows hosted in screens, and per-show seat inventory.
- Proposed eager inventory provisioning when a show is created. Example: 100 physical seats across four shows produce 400 show-seat records.
- Required both unique show/seat identity and compatibility between a show's screen and a physical seat's screen; exact enforcement remains to be designed.
- Proposed separate users, holds/hold_items, and bookings/booking_items so multi-seat selections and purchases are explicit aggregates with preserved membership and price snapshots.
- Distinguished historical hold membership from current allocation, and temporary holds/payment attempts from confirmed bookings. Payment/idempotency modelling is next.
- Saved the conceptual ER diagrams, reasoning, open issues, and study notes in `phase-2-data-model.md`. These are proposals, not a finalized schema or a passed checkpoint.

### 17 October 2026 — payment records and catalogue schema

- Captured the payment-attempt versus client-operation distinction, scoped atomic idempotency claims, request fingerprints, persisted gateway keys, and retry behavior after definitive failure versus uncertain outcome.
- Proposed schema conventions: UUID entity IDs, required-field NOT NULL constraints, TIMESTAMPTZ instants, IANA locality timezones, and integer minor-unit prices (initial currency INR).
- Specified working catalogue columns and core PK/FK/UNIQUE/CHECK rules in the data-model document.
- Proposed retaining screen_id in show_seats with composite foreign keys to enforce that physical seats and shows share the screen. Recorded the controlled redundancy and associated constraint/index cost.
- Show scheduling overlap, complete inventory provisioning, transactional schema, normalization, indexes, and Phase 2 checkpoint remain open. No SQL migrations or application code have been written.

### 17 October 2026 — transactional schema proposal

- Added concrete working columns for users, holds/items, bookings/items, payment attempts, request idempotency, and current show-seat allocation.
- Proposed direct guardrails: unique booking per hold and payment attempt, unique confirmed-booking item per show-seat, scoped request keys, and unique gateway keys.
- Proposed partial unique indexes for one live hold per user/show and one pending/unknown/successful payment attempt per hold. Requests must retire expired ordinary holds transactionally; time-based validity is not an automatically updating index predicate.
- Distinguished row-level constraints from transaction invariants: full seat-set acquisition, item counts, totals, ownership/show compatibility, and coordinated transitions. Listed remaining composite-key and recovery work explicitly.
- Updated study notes. Phase 2 has not been marked complete and no implementation has started.
