<!-- STAND-IN: replace with the findings from the module 7 authoring run. -->
# Trust boundary audit: itinerary API

Files: `booking_system_backend/server.py`, `booking_system_backend/schemas.py`, `booking_system_backend/services/itinerary.py`. Line numbers refer to the code before the fix.

## Phase 1: Trust boundary

| Endpoint | Client-controlled fields | What the server does with them |
|---|---|---|
| `POST /itineraries` | `user_id`, `name`, `flight_ids`, `quoted_total_price` | Validates the user and flights, holds every leg, and stores `quoted_total_price` as the trip's total when it is present |
| `POST /itineraries/{itinerary_id}/confirm` | `itinerary_id` | Confirms the holds of a held itinerary |

## Phase 2: Controls

| Control | Result |
|---|---|
| V5.1.2 Mass parameter assignment | FAIL |
| V5.1.3 Allow-list input validation | FAIL |
| V11.1.4 Limits on excessive business requests | FAIL |
| V11.1.5 Business logic limits | FAIL |

## Phase 3: Findings

**Finding 1**
- Rule: ASVS V11.1.5
- Severity: High
- File and line: `booking_system_backend/services/itinerary.py`, line 172
- Issue: The server stores the client-supplied `quoted_total_price` as the itinerary's total price.
- Fix: Compute the total from the flights' prices on the server and ignore any client value.

**Finding 2**
- Rule: ASVS V5.1.2
- Severity: High
- File and line: `booking_system_backend/schemas.py`, line 113
- Issue: `ItineraryCreate` accepts `quoted_total_price` and ignores unknown fields, so clients can set server-owned values.
- Fix: Remove the field and reject undefined request fields with HTTP 422.

**Finding 3**
- Rule: ASVS V11.1.4
- Severity: Medium
- File and line: `booking_system_backend/services/itinerary.py`, line 158
- Issue: A user can create any number of held itineraries and keep seats on hold.
- Fix: Limit each user to 3 held itineraries created within the hold duration and return HTTP 429.
