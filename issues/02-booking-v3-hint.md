# Booking-v3 date hint conflicts with day-first tenants — 33 reports

## Problem

The booking-v3 form shows `MM/DD/YYYY` for tenants whose locales use day-first dates. People follow the form hint, but the form reads their input using the tenant's day-first order. The month and day can be reversed. A person intending 6 January can enter `01/06` from the hint, which the form reads as 1 June.

## Evidence from ticket #4471

- 33 unique reports across alder.physio, copper.studio and pale.harbour, from 2026-04-17 through 2026-09-22.
- All 33 reports came through the form channel.
- Intake tagged 18 reports `hint` and 15 `parser`; the intake tag is only a guess.
- Example: report R-41230 from copper.studio (`es-ES`, `booking-v3` form) says a client arrived on 1 June instead of 6 January.

The export has 427 rows but 15 repeated report IDs. Counts above include each ID once, for 412 unique reports total.

## Expected behavior

Make the date hint and form parser use the same date order. If the form shows `MM/DD/YYYY`, it must parse the value month first; if it parses day first, the hint must show day first.
