# Changelog

## 2026.11.03 — first public release

- 25 profiled factory records: 20 PCB, 5 wire harness, across 4 clusters
  (Suining, Kunshan, Yangtze Delta, Pearl River Delta).
- 3 screened-out records: one ST-flagged listed company, one plant with production
  equipment in judicial auction, one plant declared bankrupt in 2024. Published as
  records rather than profiles, with the reason in `status_note`.
- 408 field-level rows with source notes; 124 external source URLs.
- Two headcount series kept separate: `insured_headcount` (social-insurance
  contributors, 19 of 25) and `group_headcount` (employees, 13 of 25). The gap on the
  8 records publishing both runs from 0.8x to 26.3x.
- `parent_ticker` added for the one plant that is a subsidiary of a listed group,
  so its parent's code is never mistaken for the plant's own.
- Cluster totals carry a coverage ratio (`n/m`) and are left blank when a member is
  missing the figure, rather than summing a partial set.
