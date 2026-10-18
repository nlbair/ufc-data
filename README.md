# UFC Data: scorecards, round statistics, and betting odds, 2013 to 2026

> This project is independent. It is not affiliated with, endorsed by, or sponsored by the UFC. UFC and related marks belong to their owners.

Fight-level and judge-round data for UFC fights from 2013-02-16 through 2026-10-10. It links each judge's round-by-round scorecard to round statistics, betting odds, official rankings at fight time, and fighter attributes, so you can study how judges score rounds and how betting markets price fights.

Version 0.1.0, released 2026-10-18. This release contains data and documentation only. The collection and build code is not published.

## What is in the dataset

| File | What it is | Size |
| --- | --- | --- |
| `data/ufc_all_fights_wide.csv` | One row per fight, every UFC fight in the window | 6,743 fights, 495 columns |
| `data/ufc_judge_rounds.csv` | One row per judge per round, for decisions with round-by-round scorecards | 29,937 rows, 69 columns |
| `data/id_crosswalk_all.csv` | Links each fight to its source IDs and records how well each match fit | 6,743 fights |
| `data/judge_names.csv` | Judges by MMA Decisions ID, with the number of cards scored | 377 judges |
| `data/jurisdiction_lookup.csv` | Event location to jurisdiction mapping used in the data | one row per location |
| `data/samples/` | Small previews of the files above | about 100 fights |
| `codebook/` | Every column explained: meaning, group, source, type, share missing | one row per column |

Of the 6,743 fights, 3,303 went to the judges and 3,440 ended another way. 3,189 decisions have round-by-round scorecards (3,125 with all three judges). 6,705 fights (99.4%) have betting odds.

## Loading the data

Python: `pd.read_csv("data/ufc_all_fights_wide.csv", low_memory=False)`

R: `readr::read_csv("data/ufc_all_fights_wide.csv")`

## Key ideas

- Fighter 1 and fighter 2 follow the order on ufcstats, which tends to list the winner first, so fighter 1 wins more often than fighter 2. For modeling, use the randomly chosen primary fighter in the judge-round file instead.
- Round scores are given as published, after any point deduction, and with the deducted point added back (the `raw` columns), which shows who the judge thought won the round.
- Win probabilities are computed from the American moneyline and have the bookmaker's margin removed.
- A blank rank means unranked, not missing. The rank is the official rank on the last weekly board before the event, and 0 is the champion.
- `finish_type` is decision, ko_tko, submission, dq, overturned, could_not_continue, or other.

## Known limitations

- There is no measure of damage. Statistics are strikes, takedowns, knockdowns, control time, and submission attempts as counted by ufcstats.
- Scorecards exist only for decisions, and round-by-round scores are missing for 114 of them, because the source lists only card totals or has no page.
- Rankings begin in February 2013, so the window starts there.
- Odds are missing for 38 fights. The opening line is the first line listed by BestFightOdds and may not have been available to bet at that time.
- Event dates can differ by a day between sources, especially for events outside the United States, and matching allowed for that.
- The data were assembled from public sources by matching names, dates, and card totals. See METHODS.md for how, and for the checks that were run.

## Sources and credit

This dataset is built from the sources listed in SOURCES.md, which should be credited when you use it: ufcstats.com, MMA Decisions, BestFightOdds, a public archive of the official UFC rankings, and the UFCalendar rankings API.

## How to cite

See CITATION.cff, or use the DOI shown on the Zenodo record once it is assigned.

## License

The data are released under CC BY 4.0. See LICENSE.

## Disclaimer

This project is independent. It is not affiliated with, endorsed by, or sponsored by the UFC. UFC and related marks belong to their owners.
