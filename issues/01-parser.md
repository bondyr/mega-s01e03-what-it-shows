# Shared parser reads day-first dates as month-first — 379 reports

## Problem

The shared date parser reads every date month first. In day-first locales, it can reverse the month and day. This cause covers app and email reports, plus form reports from booking-v2.

## Evidence from ticket #4471

- 379 unique reports across 11 tenants, from 2026-04-01 through 2026-10-04.
- Channels: 204 app, 150 email and 25 form.
- Intake tagged 356 reports `parser` and 23 `hint`; the intake tag is only a guess.
- Example: report R-41200 from Harbour Dental (`en-GB`, app) says a 7 April reminder went out as 4 July.
- Five reports noted as one-day moves still classify under this parser cause by tenant and form.

The export has 427 rows but 15 repeated report IDs. Counts above include each ID once, for 412 unique reports total.

## Expected behavior

For the shared parser path, parse each date in the order expected by the tenant and preserve its intended calendar date.
