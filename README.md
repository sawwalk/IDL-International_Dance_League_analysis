# IDL Scoresheet Spread — Dataset README

## What this is
One row per **team performance** across the four completed 2026 International Dance League (IDL) Pro Division series: **New York, Vancouver, Sydney, Seoul**. Source: [idl.pro/results](https://www.idl.pro/results) (as of Sep 4, 2026 — the season is ongoing; Los Angeles and the Championship had not yet occurred at time of collection).

36 rows total = 4 series × 9 performances (6 Round 1 performances + 3 Final performances per series).

---

## Column definitions

| Column | Description |
|---|---|
| `team` | Team name (see Naming conventions below) |
| `series` | Which event: New York / Vancouver / Sydney / Seoul |
| `round` | `Round 1` (standard 1v1 matchup) or `Final` (top 3 teams from each series' three Round 1 matches) |
| `opponent` | One team name if `round` = Round 1; two comma-separated team names if `round` = Final |
| `total_score` | Sum of the team's 10 average criteria scores (see Score types below) |
| `criteria_1_...` through `criteria_10_...` | Each team's average score (mean across the 6-judge panel) for that criterion, on the site's 0–10 scale. Column names embed the criterion label in lowercase/underscore form (e.g. `criteria_1_complexity_of_choreography`) |
| `fan_vote_pct` | Live fan vote share (%) received by that team in that performance |
| `fan_vote_count` | Raw fan vote count, where available (see Structural nulls) |
| `round1_points` | Round 1 only. Points awarded 0–7 (see Score types below). Blank on Final rows. |
| `round2_final_score` | Final only. Official fan-vote-adjusted placement score used to rank 1st/2nd/3rd. Blank on Round 1 rows. |

---

## Score types — three different numbers, don't conflate them
1. **`total_score`** — pure sum of the 10 judge-panel criteria averages. This is a measure of judged performance only, unaffected by fan voting.
2. **`round1_points`** — the head-to-head score (max 7) shown on the site for Round 1 matchups. Total across both teams in a match always sums to 7 (e.g. 3–4, 7–0, 6–1), consistent with a 6-judge majority-pick system plus 1 point for the fan-vote winner.
3. **`round2_final_score`** — the official Final-round placement score (e.g. 99.28, 89.97, 86.25) used to determine 1st/2nd/3rd in the series. This is **not** the same as `total_score` for the same row — it includes a fan-vote bonus on top of the judged criteria sum. Example: Brotherhood's New York Final `total_score` = 98.50, but `round2_final_score` = 99.28.

Use `total_score`/`criteria_*` for judge-only analysis; use `round1_points`/`round2_final_score` for official-outcome analysis.

---

## Structural nulls (expected, not missing data)
- **`round1_points`** is blank for all `Final` rows — this field only applies to Round 1 head-to-head matches.
- **`round2_final_score`** is blank for all `Round 1` rows — this field only applies to Final-round placements.
- **`fan_vote_count`** is blank for 4 rows: **Seoul Round 1**, both matches (Brotherhood vs Quick Style, Jam Republic vs 1Million). The source page displayed only fan-vote *percentages* for these two matches, not raw vote counts. This is a genuine gap in the source data, not a transcription omission.

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
- **Match 2, Match 3, and Final** scoresheets could not be retrieved automatically — the source site loads them client-side via JavaScript tab switching, which a static page fetch cannot trigger. These were manually copied from the live site by the user and transcribed into the dataset.
- Every manually-provided match was cross-checked: each team's 10 criteria values were summed and confirmed to match the "total" figure displayed alongside it on the site, catching any transcription errors before inclusion.
- One duplicate paste (Vancouver Match 1 resubmitted as "Match 2") was caught via this same cross-check and corrected before inclusion.

## Known limitations
- Reflects only the 4 completed series as of collection date. Los Angeles and the Championship series are excluded (not yet run).
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


## Dashboard Prompt
Initial prompt:

AIM: Create a custom html artifact that serves as a dashboard for teams competing in the IDL.

The convention I am using to design the document is naming each section (followed by styling constraints in brackets): the actual text that I want to be displayed. I may use markdown syntax to specify other formatting options within the text. text is square brackets [] should be filled out yourself.

Here is the structure starting from the top down:

TITLE (middle indent): International Dance League Team Scoresheet Analysis

SUBTITLE (grey font, middle indent): By Samuel Walker

DATE (left indent, use format dd mmm yyyy e.g. 5 Aug 2026): Last Updated: [todays date]

DISCLAIMER: **DISCLAIMER:** This document is meant for personal interest only. It’s purpose is not to demonstrate another team’s superiority over another but to provide an engaging way of viewing how teams performances are mapped to IDLs scoring criteria by the current judges.

MASTER GRID: [3 by 2 grid of each team’s radar chart, color coded, showing avg scores across the entire series so far]

INDIVIDUAL VIZ (large, center indent): [color coded radar chart showing average performance for each team with team filter toggles, just like the one you created for me previously with added filters that allows you to segment by series and round]

AI DILIGENCE STATEMENT: [acknowledge your own part in this project, the goal is for me to acknowledge AI's involvement and to state that I have reviewed the AI output and that I take responsibility for this work.]

Please ask me questions until you have a perfect understanding of what I am trying to design.

Iteration 1 prompt:  

This looks fantastic. I would like to fine tune and add some things.

INDIVIDUAL VIZ - modify the default display so that it compares 1M and RF instead of BH and JR, since this closer comparison is more in-keeping with the disclaimer. 

MASTER GRID - I can see that the criteria is not displayed on the radar chart but shows up when I mouse over. However this is proving to be unreliable.Lets change to a 2 wide 3 high grid so that we can enlarge each chart and inlcude the criteria names on the radar chart nodes, I understand that the viewer will have to scroll to see the full grid on a laptop.

I would also like to try the light theme.
