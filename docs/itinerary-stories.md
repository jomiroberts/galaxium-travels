<!-- STAND-IN: replace with the stories from the module 3 authoring run. -->
# Multi-leg itineraries: user stories

Travelers heading to the outer moons can book only one flight at a time today. These stories cover booking one trip that connects up to three flights.

Assumptions: economy seats only; travelers can't change a trip after they book it; travelers search by origin, destination, and departure date.

## 1. Search for connecting routes

As a traveler, I want to search for routes of up to three connecting flights, so that I can reach destinations with no direct flight.

Acceptance criteria:
- Searching Earth to Europa on 2099-02-01 returns one route with three legs: Earth to Mars, Mars to Jupiter, and Jupiter to Europa.
- Searching Earth to Mars on 2099-02-01 returns the direct flight first, then the two-leg route via Moon, because results are sorted by total travel time.
- Searching "earth" to "EUROPA" returns the same result as "Earth" to "Europa".
- Searching Earth to Earth returns an error that says origin and destination are the same.

## 2. Only offer connections I can make

As a traveler, I want every connection to leave enough time but not strand me, so that I don't miss a flight or wait for days.

Acceptance criteria:
- A layover of exactly 2 hours or exactly 72 hours is offered.
- A layover of 1 hour 59 minutes or 72 hours 1 minute is not offered.
- A route never visits the same place twice, and a leg with no economy seats left is never offered.

## 3. Book the whole trip or nothing

As a traveler, I want to book every leg of a trip in one step, so that I never end up holding half a trip.

Acceptance criteria:
- Booking the Earth to Europa route on 2099-02-01 holds all three legs and returns a trip with status held.
- If the third leg can't be held, the first two holds are released and no trip is saved.
- If confirming the second leg fails, the confirmed first leg is cancelled, its seat is returned, and the trip's status is failed.

## 4. See my trips

As a traveler, I want to see the trips I booked, so that I can check my itinerary.

Acceptance criteria:
- After booking the Earth to Europa route, My trips lists the trip with status confirmed and all three legs in order.
- My trips shows only my own trips, newest first.

## Out of scope

- Changing or cancelling a booked trip
- Seat classes other than economy
- Round-trip search
