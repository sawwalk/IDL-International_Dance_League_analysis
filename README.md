# IDL Scoresheet Spread — Dataset README

## What this is
One row per **team performance** across the four completed 2026 International Dance League (IDL) Pro Division series: **New York, Vancouver, Sydney, Seoul**. Source: [idl.pro/results](https://www.idl.pro/results) (as of Sep 4, 2026 — the season is ongoing; Los Angeles and the Championship had not yet occurred at time of collection).

54 rows total = 6 series × 9 performances (6 Round 1 performances + 3 Round 2 performances per series).

---

## Column definitions

| Column | Description |
|---|---|
| `team` | Team name (see Naming conventions below) |
| `series` | Which event: New York / Vancouver / Sydney / Seoul |
| `round` | `Round 1` (standard 1v1 matchup) or `Round 2` (top 3 teams from each series' three Round 1 matches) |
| `opponent` | One team name if `round` = Round 1; two comma-separated team names if `round` = Round 2 |
| `total_score` | Sum of the team's 10 average criteria scores (see Score types below) |
| `criteria_1_...` through `criteria_10_...` | Each team's average score (mean across the 6-judge panel) for that criterion, on the site's 0–10 scale. Column names embed the criterion label in lowercase/underscore form (e.g. `criteria_1_complexity_of_choreography`) |
| `fan_vote_pct` | Live fan vote share (%) received by that team in that performance |
| `fan_vote_count` | Raw fan vote count, where available (see Structural nulls) |
| `round1_points` | Round 1 only. Points awarded 0–7 (see Score types below). Blank on Round 2 rows. |
| `round2_final_score` | Round 2 only. Official fan-vote-adjusted placement score used to rank 1st/2nd/3rd. Blank on Round 1 rows. |

---

## Score types — three different numbers, don't conflate them
1. **`total_score`** — pure sum of the 10 judge-panel criteria averages. This is a measure of judged performance only, unaffected by fan voting.
2. **`round1_points`** — the head-to-head score (max 7) shown on the site for Round 1 matchups. Total across both teams in a match always sums to 7 (e.g. 3–4, 7–0, 6–1), consistent with a 6-judge majority-pick system plus 1 point for the fan-vote winner.
3. **`round2_final_score`** — the official Round 2 placement score (e.g. 99.28, 89.97, 86.25) used to determine 1st/2nd/3rd in the series. This is **not** the same as `total_score` for the same row — it includes a fan-vote bonus on top of the judged criteria sum. Example: Brotherhood's New York Round 2 `total_score` = 98.50, but `round2_final_score` = 99.28.

Use `total_score`/`criteria_*` for judge-only analysis; use `round1_points`/`round2_final_score` for official-outcome analysis.

---

## Structural nulls (expected, not missing data)
- **`round1_points`** is blank for all `Round 2` rows — this field only applies to Round 1 head-to-head matches.
- **`round2_final_score`** is blank for all `Round 1` rows — this field only applies to Round 2 placements.
- **`fan_vote_count`** fan vote counts are not available during Seoul Round 1 and the entirity of Los Angeles series and the championship. The site only displays percentages for those rounds.
- **fan_vote_pct** in Championship Round 2: fan vote percentages were not displayed at all for this round since fan vote does not affect the final scores in the championship Round 2 as opposed to the Round 2 in all other series.

---

## Flagged inconsistency (source-data issue, not ours)
- **Sydney, Round 1, Quick Style vs 1Million**: the site displays fan vote shares of **11% (Quick Style) / 81% (1Million)**, which sum to 92%, not 100%. This looks like a data-entry error on idl.pro itself. Recorded as-is (unadjusted) in the CSV. If this row matters to your analysis, worth flagging or excluding.


---

## Naming conventions
- **Column headers** are standardized to lowercase, underscore-separated (`snake_case`) — no spaces, hyphens, or slashes. E.g. `fan_vote_pct`, `criteria_9_projection_communication`. The one exception is `+` in criterion 7's original name ("Technical Execution + Authenticity"), rendered as `criteria_7_technical_execution_and_authenticity`.
- **Team names** used exactly as branded on-site: `Brotherhood`, `GRV`, `1Million`, `Royal Family`, `Jam Republic`, `Quick Style`. (Home markets, for reference: Brotherhood – Vancouver, CAN; GRV – Los Angeles, USA; 1Million – Seoul, KOR; Royal Family – Auckland, NZL; Jam Republic – South East Asia, SEA; Quick Style – Oslo, NOR.)
- **Series names** = host city only (`New York`, `Vancouver`, `Sydney`, `Seoul`), matching the site's results URLs (`idl.pro/results/<series>`).
- **Criteria names** are copied verbatim from the site's "SCORESHEET SPREAD" panel and embedded in each criteria column header for self-description.

---

## Collection method (for auditability)
- Each series' **Match 1** scoresheet was pulled automatically via direct page fetch (this is the default-rendered tab on each results page).
- **Match 2, Match 3, and Round 2** scoresheets could not be retrieved automatically — the source site loads them client-side via JavaScript tab switching, which a static page fetch cannot trigger. These were manually copied from the live site by the user and transcribed into the dataset.
- Every manually-provided match was cross-checked: each team's 10 criteria values were summed and confirmed to match the "total" figure displayed alongside it on the site, catching any transcription errors before inclusion.
- One duplicate paste (Vancouver Match 1 resubmitted as "Match 2") was caught via this same cross-check and corrected before inclusion.

## Known limitations
- Per-judge (individual judge, not just team average) scores were not captured in this dataset — only the panel-average per criterion. If judge-level granularity is needed later, it is available on the source pages but would require additional extraction.

---

# AI prompts and context:

TEAM ABREVIATIONS:
BH: Brotherhood
GRV: GRV
1M: 1Million
Quick: Quick Style
JR: Jam Republic
RF: Royal Family


TEAM CHARACTERISTIC COLORS:
BH: red
GRV: green
1M: grey
Quick: light blue
JR: orange
RF: gold