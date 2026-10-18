# Methods

This file describes how the data were built and how they were checked. The collection and build code is not published.

## Scope

Every UFC fight listed on ufcstats from 2013-02-16 through 2026-10-10. The window starts when the weekly official rankings start, because a fight needs a ranking board from before it took place.

## Where each piece comes from

- Fight list, results, round and whole-fight statistics, judges' card totals, and fighter attributes: ufcstats.com.
- Each judge's round-by-round scores and point deductions: MMA Decisions. Each ufcstats decision was matched to an MMA Decisions page on event date (allowing one day of difference), fighter names, and the three judges' card totals.
- Opening lines and closing ranges: BestFightOdds. Each fight was read from one fighter's page, with the opponent's page used when the first did not match, and matched on the fighter's own page and a date within two days.
- Rankings: a public archive of weekly UFC rankings through June 2026, and the UFCalendar rankings API after that. The rank used is on the last weekly board strictly before the event.

## Derived variables

- Win probabilities: each moneyline is converted to an implied probability, and the two are divided by their sum so they add to one. The closing probability uses the midpoint of the closing range.
- Pre-deduction scores: the deducted point is added back to the penalized fighter in the round of the deduction. The published scores sum to the card totals.
- Age: years from the fighter's date of birth to the event date. Date of birth is not published in this release.
- Primary fighter: chosen at random for each fight, with a fixed seed, so differences in the judge-round file are signed to a randomly chosen side.
- Previous round and lead: the previous-round winner uses the judge's pre-deduction scores. The running lead uses published scores. The insurmountable-lead flag marks a lead of at least two points for each remaining round, and the raw ingredients are included so other definitions can be applied.
- Rule eras: flags for events on or after 2017-01-01 and 2025-08-06.
- Jurisdiction: the US state, Canadian province, or Australian state of the event, and the country elsewhere, from the table in `data/jurisdiction_lookup.csv`.
- Names: nine ranked fighters whose ranking-list names differ from ufcstats were linked by hand, and one pair of fighters sharing a name was separated by ID.

## Checks at release

- Of the decision fights in the window, 3,296 of 3,303 were matched to an MMA Decisions page. In every one of the 3,248 matches with a winner on both sites, the same fighter won.
- In 3,284 of those matches all three card totals equal ufcstats'; in 12 one card differs by about a point.
- In every fight, the per-round statistics add up to the whole-fight totals on the headline measures checked.
- The same fight read from two fighters' pages on BestFightOdds gave identical odds in 825 of 827 cases.
- On 2026-06-02, the two ranking sources agreed on all 206 rows. Three boards with duplicated fighters are excluded from rank lookups.

## Matters to keep in mind

- Matching is not perfect, and the crosswalk records how each match fit so that weak matches can be found and excluded.
- Where a source lists no value, the cell is blank and not zero.
- Judges are identified by MMA Decisions IDs; the same person can be spelled differently on ufcstats.
