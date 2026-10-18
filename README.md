# Cookie Cats A/B Test: Does Gate Placement Affect Player Retention?

## The Question
Cookie Cats is a mobile puzzle game with a paywall ("gate") that 
blocks players at a certain level. This project tests: **does 
moving the gate from level 30 to level 40 change whether players 
keep coming back?**

##  Why Does This Matter
Small product decisions like where a paywall sits can quietly 
help or hurt long-term engagement. A/B testing is how companies 
make that call with evidence instead of guesswork. I built this 
project because A/B testing shows up in almost every data analyst 
job posting, and I wanted real, hands-on experience with it rather 
than just a definition.

## Step 1: Framing the Test
- Two groups, randomly assigned when players installed the game: 
  *gate_30* (unchanged) and *gate_40* (gate moved later)
- Two metrics: *1-day retention* and *7-day retention* 
  (did the player come back?)
- Hypothesis: gate placement has no effect on retention, unless 
  the data says otherwise

## Step 2: Building the Test by Hand in Excel
I deliberately did this in Excel first, before any code, so I'd 
actually understand the statistics instead of just calling a 
function.

- Checked the group split was roughly even (sample ratio check) — 
  44,700 vs 45,489, confirmed clean
- Calculated retention rate per group
- Built a *two-proportion z-test* manually: pooled proportion → 
  standard error → z-score → p-value

*Problems I ran into and fixed:*
- #DIV/0! errors — traced to the retention columns being read 
  as text ("TRUE"/"FALSE") instead of real numbers. Fixed with a 
  helper column (=IF(D2,1,0)) to force real 1s and 0s.
- A p-value of exactly 0 made no sense, since that's statistically 
  very unlikely. Traced it to a misplaced parenthesis in my 
  standard error formula, which silently broke the square root 
  calculation.

*Final Excel results:*
- 1-day retention p = 0.074
- 7-day retention p = 0.00155

## Step 3: Confirming in Python
I rebuilt the same test in Python (pandas, scipy, statsmodels) to 
check my manual Excel work against a trusted library.

- Loaded the CSV, grouped by version, re-ran the z-test with 
  proportions_ztest
- Hit a separate problem along the way: my script file got 
  corrupted through a chain of renaming/re-saving and ended up 
  empty. Instead of continuing to debug the file, I pasted the 
  code directly into a Jupyter cell and ran it there.
- Python results matched Excel almost exactly: 1-day p = 0.0744, 
  7-day p = 0.0016 — confirming the manual calculations were correct.

## Step 4: Building the Power BI Dashboard
- Imported the same dataset into Power BI
- Built DAX measures for retention rate by group
- Created a clustered column chart comparing 1-day and 7-day 
  retention across both versions
- Added KPI cards for quick headline numbers
- Added text panels summarizing the statistical results and the 
  final recommendation

## Results

| Metric | gate_30 | gate_40 | p-value | Real difference? |
| 1-day retention | 44.8% | 44.2% | 0.074 | No |
| 7-day retention | 19.0% | 18.2% | 0.0016 | *Yes* |

## The Insight
Short-term behavior looked basically identical between groups. 
A week later, a real gap appeared and it favored the *earlier* 
gate. Giving players more free levels before a paywall didn't 
help retention; it quietly hurt it, likely by letting players 
burn through content too fast and lose engagement sooner.

## Recommendation
*Keep the gate at level 30.* Moving it to level 40 offers no 
measurable short-term benefit and risks losing long-term engaged 
players.

## Tools Used
Excel · Python (pandas, scipy, statsmodels) · Power BI

## What's In This Repo
- cookie_cats.csv — the dataset
- cookie_cats_analysis.xlsx — statistical calculations done by hand
- ab_test_analysis.ipynb — Python analysis and confirmation
- dashboard.png — Power BI dashboard screenshot

  Full write up on my medium page : https://medium.com/@ifechukwuokoli97/from-pharmacy-to-p-values-my-first-a-b-testing-project-548f67d6bc13

## 🔗 More
Full write-up with the debugging process: [Medium article link]
