# Task-Oriented Augmented Reality Experiment Data

This repository contains the analysis data and reproducible R report for *An Information Analysis Framework for Task-Oriented Augmented Reality Interface Design in Drone Operations*.

## Experiments

| ID | Evaluation | Task and setting | Participants |
|---|---|---|---:|
| E1 | Initial interface evaluation | Obstacle avoidance, real system | 20 |
| E2a | Exploratory TPV AR cue design evaluation | Obstacle avoidance, simulator | 6 |
| E2b | Second interface evaluation | Obstacle avoidance, real system and simulator | 16 |
| E3 | Matrix-guided interface evaluation | Target search, real system | 20, same participants as E1 |

The four interface conditions are Basic (no AR), FPVAR (AR in the first-person view), TPVAR (AR in the third-person view), and BothAR (AR in both views). E2a compares three TPV cue designs, LS (Line and Shadow), CC (Concentric Circles), and CP (Compass).

## Files

| File | Contents |
|---|---|
| [1,3_task performance.xlsx](1%2C3_task%20performance.xlsx) | E1 and E3 task performance. The `ObstacleAvoidance` sheet contains completion time and collision count. The `TargetSearch` sheet contains completion time and target revisit count. |
| [1,3_eyetracking.xlsx](1%2C3_eyetracking.xlsx) | E1 and E3 eye-tracking summaries, with one row per participant, interface, and view. The `collision` and `target` sheets contain total duration, proportion, and mean fixation duration. |
| [1,3_NasaTLX.xlsx](1%2C3_NasaTLX.xlsx) | Weighted NASA-TLX scores for E1 and E3 in the `collision` and `target` sheets. |
| [1,3_SUS.xlsx](1%2C3_SUS.xlsx) | System Usability Scale (SUS) scores for E1 and E3, with item responses in the `raw data` sheet. |
| [2_cueDesign.xlsx](2_cueDesign.xlsx) | E2a completion time, mean and total warning-zone dwell time, and SUS scores. |
| [2_performance.xlsx](2_performance.xlsx) | E2b completion time, collision count, mean warning-zone dwell time, NASA-TLX, SUS, and trial-level eye-tracking summaries. |
| [2_eyetracking.xlsx](2_eyetracking.xlsx) | E2b fixation durations in long format, with one row per participant, environment, interface, and view. |
| [experiment sequence.xlsx](experiment%20sequence.xlsx) | Supplementary experiment sequence workbook. |
| [Drone_AR_Analysis.Rmd](Drone_AR_Analysis.Rmd) | R Markdown analysis source. |
| [Drone_AR_Analysis.html](Drone_AR_Analysis.html) | Rendered analysis steps, descriptive statistics, and test results. |

The current second-evaluation inputs are `2_performance.xlsx` and `2_eyetracking.xlsx`. Use these filenames when reproducing the analysis.

## Outcomes and statistical analyses

| Evaluation | Outcome | Main analysis | Follow-up or planned comparison |
|---|---|---|---|
| E1 | Completion time, collision count | Friedman test across interfaces | Paired Wilcoxon signed-rank tests across interfaces after a significant omnibus test |
| E1 | TPV fixation proportion | Friedman test across interfaces | Paired Wilcoxon signed-rank tests across interfaces after a significant omnibus test |
| E1, E3 | Fixation proportion by view | Paired t-test comparing FPV and TPV within each interface | Four view comparisons, adjusted together |
| E1, E3 | Mean fixation duration by view | Paired Wilcoxon signed-rank test comparing FPV and TPV within each interface | Four view comparisons, adjusted together |
| E1, E3 | Weighted NASA-TLX, SUS | One-way repeated-measures ANOVA across interfaces | Paired t-tests across interfaces after a significant omnibus test |
| E2a | Completion time | Friedman test across LS, CC, and CP | Paired Wilcoxon signed-rank tests across cues after a significant omnibus test |
| E2a | Mean warning-zone dwell time, SUS | One-way repeated-measures ANOVA across cues | Paired t-tests across cues after a significant omnibus test |
| E2a | Total warning-zone dwell time | Descriptive statistics by cue | — |
| E2b | Completion time, collision count, mean warning-zone dwell time, TPV fixation proportion, SUS | Two-way aligned rank transform (ART) ANOVA, environment × interface | Pairwise ART contrasts with ART or ART-C alignment as selected by `art.con()` |
| E2b | Total and mean fixation duration | Three-way ART ANOVA, environment × interface × view | Pairwise ART contrasts with ART or ART-C alignment as selected by `art.con()` |
| E2b | Weighted NASA-TLX | Two-way repeated-measures ANOVA, environment × interface | Paired t-tests for significant main effects, averaging over the other factor |
| E2b | Relative effects on completion time, TPV fixation proportion, NASA-TLX, and SUS | Within-participant AR-minus-Basic differences estimated separately by environment | Pointwise 95% t confidence intervals |
| E3 | Completion time, target revisit count, FPV fixation proportion | Friedman test across interfaces | Paired Wilcoxon signed-rank tests across interfaces after a significant omnibus test |

### Follow-up comparisons and reporting

- **Multiplicity.** Bonferroni adjustment is applied to the pairwise tests within each outcome and comparison family. One-way interface comparisons have six pairs, E2a cue comparisons have three pairs, and within-interface FPV–TPV tests have four comparisons per eye-tracking outcome. In E2b, the report adjusts by outcome, focal factor, and scope.
- **ART follow-ups.** For factors involved in a significant interaction, comparisons hold the other factors fixed. Other significant main effects use marginal comparisons. The report also shows the overall real-versus-simulated contrast for collision count.
- **Test settings.** Tests use a significance level of .05. Repeated-measures ANOVA uses Greenhouse–Geisser correction when Mauchly's test indicates a sphericity violation. Wilcoxon signed-rank tests use the normal approximation with continuity correction. ART models include a participant random intercept.
- **Effect sizes.** The report gives Kendall's W for Friedman tests, partial eta squared for repeated-measures ANOVA and its F-based approximation for ART ANOVA, Cohen's dz for paired t-tests, matched-pairs rank-biserial correlation for Wilcoxon tests, and d-rank for ART contrasts.
- **Dwell time.** Trial-level mean dwell time is total time in the warning zone divided by the number of entries. Trials with no entries are recorded as 0 s.

## Reproduce the HTML report

Keep the Rmd and input workbooks in the same directory. Open the Rmd in RStudio and select **Knit to HTML**, or run the following from that directory with Pandoc available.

```r
install.packages(c("readxl", "dplyr", "ez", "ARTool", "emmeans", "knitr", "rmarkdown"))
rmarkdown::render("Drone_AR_Analysis.Rmd")
```

If the workbooks are in another directory, pass its path as `params = list(data_dir = "path/to/GitData")` to `rmarkdown::render()`. The HTML includes software versions and displays adjusted pairwise p-values in the `p_Bonferroni` column.
