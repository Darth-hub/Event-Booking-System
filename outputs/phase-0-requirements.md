# Phase 0 — requirements and capacity estimation

Status: scope discussion in progress. Capacity estimates and the phase checkpoint are pending. No schema or concurrency implementation is finalized.

## Version-one scope

- Sign up and log in; own holds and bookings belong to the authenticated user.
- Browse movies and shows, filtered by city and date; choose theatre, screen, and showtime.
- View a seat map and request a temporary hold on selected seats.
- Explicitly release a hold early when abandoning checkout or deselecting seats. Browser exit notification is best effort; server-side expiry must work without it. Exact partial-release behavior remains to be specified in API design.
- At most 10 seats per hold and one active hold per user per show. These constrain one account's usage; they do not prevent abuse across multiple accounts or shows. Rate limiting and broader abuse controls require a later design decision.
- Server computes price from authoritative seat-type pricing; client totals cannot determine the amount. The price-validity policy during a hold remains to be specified.
- Simulate successful payment, failed payment, and successful booking confirmation whose response is lost, followed by a retry. Track simulated payment outcome and booking outcome distinctly; a payment result alone does not establish that a booking was committed.
- View confirmed booking history.
- Refetch the seat map on load and after a hold conflict (planned HTTP 409). Displayed availability is advisory; the server decides whether a hold succeeds.
- Seed movies, theatres, screens, seat layouts, prices, and showtimes.

## Correctness rules

1. A seat for a given show belongs to at most one confirmed booking.
2. A show-seat has at most one valid active hold, and cannot be newly held while booked.
3. Multi-seat hold acquisition succeeds for the whole requested set or fails without acquiring a subset.
4. Only the owning user may release or confirm a hold. A new confirmation requires an unexpired hold.
5. Repeating a confirmation for the same operation must not create another booking; it must recover the previously committed outcome where applicable.
6. Booking and hold changes require authoritative, consistent decisions under concurrency. Browsing may tolerate stale data. Inability to safely establish seat ownership must fail or defer the mutation rather than permit double booking.

## Hold lifecycle

Logical show-seat states (not a schema decision):

```text
AVAILABLE → HELD → BOOKED
             ↓
         AVAILABLE (expiry or explicit release)
```

- User preference adopted: payment failure retains the hold until its original expiry so payment can be retried. Failure does not extend the deadline.
- Proposed working default from the assistant: five-minute holds. This balances checkout time and returning scarce inventory; it is a product assumption to validate, not a measured optimum. User confirmation pending.
- Expiry is enforced by the server even if the browser closes, loses connectivity, or never sends a release request.
- Late requests must not release or confirm a newer user's hold; exact safeguards belong to the core-flow phase.

## Deliberately out of version one

- Real payments, real charges, refunds, and confirmed-booking cancellation.
- Live seat-map updates through WebSockets.
- Refresh tokens and OAuth; the authentication/session approach is still undecided.
- Organizer/admin interface; initial data is seeded.
- Reviews and recommendations.

A booking-status representation can support later evolution, but adding a CANCELLED enum value alone does not implement cancellation. Cancellation policy, seat release, payment/refund handling, and concurrency rules would still need design and testing. Enum versus another representation is a data-model decision.

## Still open before completing Phase 0

- Confirm the proposed five-minute duration and ten-seat cap as product defaults.
- Define the city/date/timezone assumptions and initial catalogue size.
- Derive daily activity, peak concurrent users, request mix, booking rate, and storage growth from the 1,000–2,000-total-user audience.
- Set measurable performance targets; correctness and capacity remain unproven until testing.
- Complete a checkpoint grill and record actual answers and remaining gaps.
