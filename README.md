# Verified Chinese PCB & wire-harness supplier records

A structured export of **28 company records** — 25 profiled plants and 3 screened-out entities — from
[emssuppliersverified.com](https://emssuppliersverified.com), a sourcing directory that publishes one
15-field identity table per factory and cites a public source for every number in it.

Everything here is derived from the published profiles. No figure was estimated, interpolated or
carried over from a company brochure. **Where a field is blank, the factory does not publish it.**
That is the point of the dataset: the blanks are as informative as the values.

- **Compiled:** 2026-11-03
- **Profiles:** 25 (20 PCB, 5 wire harness) in 4 regional clusters
- **Screened out:** 3 (one ST-flagged listed company, one insolvent plant, one bankrupt plant)
- **Field-level rows:** 408 · **External source URLs:** 124
- **License:** CC BY 4.0 — attribution to `emssuppliersverified.com`

---

## Files

| File | Rows | What it is |
|---|---|---|
| `plants.csv` | 25 | One row per profiled factory. The main table. |
| `screened_out.csv` | 3 | Same schema, for entities the directory measured but refuses to profile. `status_note` says why. |
| `fields.csv` | 408 | Long format: every published field of every record, with its source note and source URL. This is where the nuance lives. |
| `sources.csv` | 124 | Every external URL cited by each profile. |
| `clusters.csv` | 4 | Cluster totals, with a coverage ratio (`n/m`) for each total. |
| `dataset.json` | — | `plants` + `screened_out` as one machine-readable object. |
| `CITATION.bib` | — | BibTeX entry. |

## Schema — `plants.csv`

| Column | Meaning |
|---|---|
| `slug`, `profile_url` | Record identifier and the human-readable page. |
| `status` | `profiled` or `screened_out`. |
| `status_note` | Reason for screening out. Empty for profiled plants. |
| `sector` | `pcb` or `wire_harness`. |
| `legal_name_en`, `legal_name_cn` | Name as printed on the Chinese business licence, plus the English string the company itself uses. |
| `uscc` | Unified Social Credit Code — the 18-character national company identifier. 25 of 25. |
| `stock_codes` | `EXCHANGE:code` for the profiled legal person itself, e.g. `SSE:603228;HKEX:03228`. 10 of 25 are listed. |
| `parent_ticker` | Set only where the profiled plant is a subsidiary and the code belongs to the listed parent (1 record: Suining Luxshare → `SZSE:002475`). |
| `founded_date` | ISO date where the registry gives one; a bare year where that is all that is published. |
| `address_en`, `city`, `province`, `country` | Registered address, from the page's structured data. |
| `website` | The domain the company controls, not a marketplace listing. |
| `industry_code` | The **registry** industry code, as printed (2–4 digits, or `C3911`). 10 of 25. Not the product category — see `industry_note`. |
| `industry_note` | The registry code in full, including a second conflicting code where two records disagree. |
| `insured_headcount` | Social-insurance contributors, from the 2025 annual filing. 19 of 25. |
| `group_headcount` | Employees — group consolidated, exchange profile, or business-registry figure. 13 of 25. |
| `group_over_insured` | `group_headcount ÷ insured_headcount`, one decimal. Present only when both exist (8 of 25). |
| `headcount_note` | The source sentence. Kept because three plants publish three different numbers on purpose. |
| `revenue_cny` | Revenue in yuan. `revenue_year` says which year. 15 of 25. |
| `revenue_year`, `revenue_basis`, `revenue_text` | The year, where the figure came from (`identity table` / `body text`), and the sentence it was read out of. |
| `identity_rows`, `numbered_fields` | How many fields that profile actually publishes (9, 14, 15, 16 or 22 — the template is not uniform). |
| `last_checked` | Date the profile's sources were last verified. |

## Two headcounts, kept apart

Chinese company data has two unrelated headcounts and they routinely differ by an order of
magnitude. This dataset never merges them:

- `insured_headcount` — people the legal entity pays social insurance for. Administrative, comparable, usually small.
- `group_headcount` — employees of the listed group or the consolidated entity. Larger, not comparable across plants.

Of the 8 records that publish both, the gap runs from **0.8× to 26.3×**:

| Plant | Insured | Group | Group ÷ insured |
|---|---:|---:|---:|
| dongshan-precision | 1,495 | 39,245 | 26.3× |
| huguang-harness | 1,099 | 12,236 | 11.1× |
| kunshan-wanyuantong | 234 | 1,409 | 6.0× |
| shennan-circuits | 6,152 | 21,211 | 3.4× |
| kunshan-wus | 7,052 | 16,512 | 2.3× |
| shengyi-electronics | 6,269 | 7,647 | 1.2× |
| yingchuangli | 1,706 | 1,829 | 1.1× |
| luxshare-suining | 1,224 | 980 | 0.8× |

A supplier shortlist built on the insured column and one built on the group column do not contain
the same factories. Any "headcount" ranking that mixes them is arithmetic on two different things.

## Registry codes do not describe the product

`industry_code` is what the business licence says, not what the factory makes. Where the two
disagree the record keeps both:

- **dongshan-precision** — `33` 金属制品业 (metal products), the 1998 stamping-shop licence, against `C3982` 电子电路制造 on a second record. The group's 2025 revenue was ¥40.125bn.
- **shennan-circuits** — `2319` 包装装潢及其他印刷 (packaging and other printing) on one record, `C3982` on another, same USCC.
- **luxshare-suining** — `C3911` 计算机制造 (computer manufacturing) for a plant whose output is cable and connectors.
- **shengyi-electronics** — `3824` 电力电子元器件制造 (power electronic components) for a PCB maker.

Filtering a supplier database by industry code therefore returns a different shortlist depending on
which record it read. The product description, not the code, is the filter that works.

## Coverage

| Field | Filled | Note |
|---|---:|---|
| `uscc` | 25 / 25 | |
| `founded_date` | 25 / 25 | |
| `city` / `province` | 25 / 25 | |
| `insured_headcount` | 19 / 25 | Five large plants publish only a registry or annual-report employee count; one filed zero because its first filing predates production |
| `revenue_cny` | 15 / 25 | 13 × 2025, 1 × 2024, 1 × 2022 |
| `group_headcount` | 13 / 25 | |
| `industry_code` | 10 / 25 | The other 15 profiles do not print one |
| `stock_codes` | 10 / 25 | One further record is a subsidiary of a listed group — see `parent_ticker` |

Summed revenue of the 15 plants that publish a figure: **¥141.3bn**. That is a sum of different
years and different scopes (group vs plant) — it is not a market size and should not be quoted as one.

## What this dataset is not

- **Not audited.** Every number is a published number, matched to the sentence it came from. Nothing was verified on site.
- **Not a ranking or a recommendation.** Plants are ordered alphabetically by slug. No supplier here is endorsed, and presence is not an endorsement.
- **Not price data.** No quotes, no MOQ, no lead times — those are not public for any of these factories.
- **Not complete.** Capacity, layer counts and certificate numbers are claimed far more often than they are published; `fields.csv` records the claim and, where relevant, the absence of a certificate number.
- **Not normalised.** Fiscal years, entity scopes and headcount definitions differ per record and are labelled per record.

## How each column is derived

Every value is read from the profile's numbered field rows (`N · Field name`) or its JSON-LD block.
Nothing is read out of marketing copy, and nothing is carried over from another record.

| Column | Extracted from | Rejected on purpose |
|---|---|---|
| `uscc`, `founded_date`, `address_*` | The USCC row and the page's JSON-LD `identifier` / `foundingDate` / `address` | A founding year inside a company history paragraph |
| `insured_headcount` | A row that says `N insured` or `N — social-insurance contributors` | A figure from a commercial database with no filing behind it |
| `group_headcount` | `N employees` / `N group` / `N on the registry extract` | Ranges (`50–99`), and `N insured employees`, which is the other column |
| `revenue_cny` | An amount anchored to the word `revenue` — `¥X revenue` or `revenue ¥X` | Net profit, gross margin, registered capital, output value, and any `revenue ceiling / target` |
| `industry_code` | The `Industry code X` anchor only | A four-digit year that happens to sit in the same cell |
| `stock_codes` | `EXCHANGE code` in the page's alias line, or an explicit `002384.SZ` style code | A code introduced as `held by` / `wholly owned` — that belongs to the parent, not the plant |
| `group_over_insured` | Arithmetic on the two columns above | — |

When the same cell holds several years, the earliest figure in reading order wins: the profiles lead
with the latest full year. Three plants publish revenue in body copy rather than in the table; those three
values are pinned to a verbatim string that must still exist on the page, so if the page changes the
value disappears instead of drifting.

All profiles are regenerated from the live site whenever a record is added or a source is re-checked;
`last_checked` per row says when. The extraction rules above are the full specification — they are
enough to re-derive any row from the linked profile by hand.

## Citation

```bibtex
@misc{emssuppliersverified2026,
  title  = {Verified Chinese PCB and wire-harness supplier records: 25 profiled plants and 3 screened-out entities},
  author = {{emssuppliersverified.com}},
  year   = {2026},
  note   = {Compiled 2026-11-03; CC BY 4.0},
  url    = {https://emssuppliersverified.com}
}
```

Plain text: *emssuppliersverified.com, "Verified Chinese PCB and wire-harness supplier records" (compiled 2026-11-03), CC BY 4.0.*

Corrections: if a figure here disagrees with the primary source, the primary source wins — send the
document and page number and the record will be corrected or blanked.

---

<sub>中文速览：25 份已建档工厂 + 3 份筛除记录，按 15 字段身份表导出。核心口径是「社保参保人数」与「集团员工数」两套人数绝不混填，营收只取 revenue 锚定的金额，查不到的字段留空而非估算。字段逐条带来源说明，原始页面在 emssuppliersverified.com。</sub>
