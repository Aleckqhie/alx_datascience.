# Data Analytics Fundamentals — Work Log

**Date:** 2026-09-10
**Covers:** Descriptive statistics, spreadsheet formulas & cell referencing, visualization category selection..

---

## 1. Descriptive Statistics

### The core question each measure answers

Two different questions get asked about any dataset, and it's easy to blur them together:
- **Central tendency** — "what's the typical value?"
- **Spread** — "how much can I actually trust that typical value to represent the data?"

A mean without a spread measure next to it is half a picture — two datasets can share the exact same mean while one is tightly clustered and the other wildly scattered.

### Measures of central tendency

| Measure | What it is | Formula | When I should reach for it |
|---|---|---|---|
| **Mean** | Arithmetic average — sum of all values divided by count | `=AVERAGE(range)` | Clean, roughly symmetric data with no extreme outliers. Highly sensitive to outliers — a single huge value drags it. |
| **Median** | The middle value once sorted | `=MEDIAN(range)` | Skewed data or data with outliers — this is the one I should default to for return distributions, which are almost never symmetric. |
| **Mode** | Most frequent value | `=MODE(range)` | Categorical data — e.g. "which regime label occurs most often" rather than continuous return values. |

**Understanding checkpoint:** the reason median is the "outlier-resistant" choice is that it only cares about *position* in the sorted order, not magnitude — one extreme value can only shift it by one rank, whereas it can drag the mean arbitrarily far.

### Measures of spread

| Measure | What it is | Formula | When I should reach for it |
|---|---|---|---|
| **Range** | Max − Min | `=MAX(range)-MIN(range)` | Quick and dirty; but only looks at the two most extreme points, so a single outlier defines the whole number. |
| **Quartiles (Q1, Median, Q3)** | Split the sorted data into four equal groups | `=QUARTILE(range, quart_num)` — `quart_num`: 0 = min, 1 = Q1, 2 = median, 3 = Q3, 4 = max | Understanding shape of distribution beyond just center. |
| **Interquartile range (IQR)** | Q3 − Q1 | `=QUARTILE(range,3)-QUARTILE(range,1)` | Spread of the "typical" middle 50%, ignoring extreme values — this is the spread equivalent of the median: robust to outliers. |
| **Variance** | Average of squared deviations from the mean | `=VAR(range)` (generic) or `=VAR.S(range)` / `=VAR.P(range)` (explicit sample/population) | Foundational — but the squared units (e.g. "returns squared") make it hard to interpret directly. |
| **Standard deviation** | √variance | `=STDEV(range)` (generic) or `=STDEV.S(range)` / `=STDEV.P(range)` (explicit sample/population) | Same units as the original data, so it's actually interpretable — this is why volatility is almost always quoted as standard deviation, not variance. |

**Understanding checkpoint:** variance squares the deviations mainly to stop positive and negative deviations from cancelling out — but that squaring is also *why* variance overweights large deviations relative to small ones. Standard deviation undoes the units problem by taking the square root back, but that overweighting of large moves is still baked in underneath. This is worth remembering — it's part of why ATR (mean absolute range) and standard-deviation-based volatility measures can tell noticeably different stories on the same data.

**Due-diligence addition — sample vs. population (`.S` vs. `.P`):** the reference card lists generic `VAR`/`STDEV`, but Google Sheets (and modern Excel) actually split each into two explicit versions:
- **`.S`** (sample) — divides by `n − 1` instead of `n`. This correction (Bessel's correction) exists because a sample tends to slightly *underestimate* the true spread of the full population it was drawn from; dividing by a smaller number nudges the estimate back up to compensate.
- **`.P`** (population) — divides by `n`, used only when the dataset genuinely *is* the entire population, not a sample of something larger.

For almost anything in the pipeline — a window of historical returns, a year of daily bars — that data is a **sample** of a much larger (effectively infinite) space of possible market outcomes, not the full population. `STDEV.S` / `VAR.S` are the statistically correct default here, not `.P`. Worth double-checking which one any existing spreadsheet or notebook calculation is actually using, since the difference is small for large `n` but not negligible for short lookback windows (e.g. a 14- or 20-period ATR-style window, where `n` vs `n-1` is a more meaningful gap).

### Where this connects to my pipeline
- Standard deviation is the backbone of GARCH/EGARCH volatility modeling I'm already using.
- IQR is a genuinely useful *robust* alternative to standard deviation for flagging outlier bars before they contaminate a regime-detection feature.
- Median vs. mean matters directly when summarizing returns by regime — a single fat-tailed session shouldn't be allowed to define the "typical" behavior of a regime.

---

## 2. Spreadsheet Formulas & Cell Referencing

### Cell referencing — relative vs. absolute

- **Relative reference** (`B1`) — shifts automatically when the formula is copied to a new cell, based on its position relative to the original. This is the default, and it's what you want when the same *logic* should apply row-by-row (e.g. "this row's high minus this row's low").
- **Absolute reference** (`$B$1`) — stays locked to the exact same cell no matter where the formula is copied. This is what you want when a formula needs to keep pointing at one fixed constant — e.g. a single risk-free rate cell, or a single lookback-window-length parameter, referenced across hundreds of rows of calculations.

**Understanding checkpoint:** the `$` doesn't mean "don't change the value" — it means "don't let this part of the *address* shift when the formula is copied." Mixing them (`$B1` vs `B$1`) locks only the column or only the row, which becomes genuinely useful once a formula needs to be copied in both directions across a grid.

### Useful functions, grouped by what they actually do

| Function | Does | Note worth remembering |
|---|---|---|
| `AVERAGE` / `AVERAGEA` | Arithmetic mean | `AVERAGEA` also evaluates TRUE/FALSE and text as numeric — easy source of a silent bug if a column has stray text in it. |
| `POW` / `POWER` | Exponentiation | Interchangeable — different names, same operation. |
| `SQRT` | Square root | Direct building block for standard deviation from variance. |
| `CEILING` / `CEILING.MATH` / `CEILING.PRECISE` | Round **up** to the nearest multiple of a specified factor | Not the same as rounding to decimal places — `CEILING(18.25, 2)` rounds to the nearest multiple of 2, not to 2 decimal places. |
| `ROUNDUP` | Round up to a number of **decimal places** | This is the decimal-place version people usually mean when they say "round up." |
| `FLOOR` / `FLOOR.MATH` / `FLOOR.PRECISE` | Round **down** to the nearest multiple of a factor | Mirror of the CEILING family. |
| `ROUNDDOWN` | Round down to a number of decimal places | Mirror of ROUNDUP. |
| `MOD` | Remainder after division | Useful for any "every Nth row" logic — e.g. flagging every 4th bar for a lower-frequency resample check. |

**Understanding checkpoint — the mistake I want to avoid:** `CEILING`/`FLOOR` and `ROUNDUP`/`ROUNDDOWN` sound like the same operation, but they round to fundamentally different things — a *multiple of a factor* vs. a *number of decimal places*. Reaching for the wrong one silently produces plausible-looking but wrong numbers, which is worse than an obvious error.

### Where this connects to my pipeline
- Absolute referencing is exactly the pattern I'd want for any spreadsheet-based sanity-check model that references a single fixed parameter (e.g., ATR lookback length) across a full price history.
- `MOD` is a clean way to hand-verify session-bucketing or resampling logic before trusting the equivalent pandas code.

---

## 3. Visualization Category Selection

### The decision framework

The reference card's real value isn't the list of chart types — it's the *decision tree* that gets you there. Every choice starts from one question: **what relationship am I actually trying to show?**

- **Comparison** — how do items or periods stack up against each other? → bar charts (vertical for few categories, horizontal for many/long labels), line charts (over many periods), variable-width bar charts (when a second variable needs encoding in bar width).
- **Composition** — how do the parts make up the whole? → first split on *static snapshot vs. changing over time*, then on *how many periods/components*. Pie charts and 100% stacked bars for a simple share-of-total; waterfall charts when accumulation/subtraction to a total matters; tree maps when there are subcomponents within components.
- **Distribution** — how are the individual data points spread out? → histograms (bar or line depending on data volume) for a single variable.
- **Relationship** — how do two or more variables relate to each other? → scatter plots for two variables, bubble charts once a third variable needs encoding (via bubble size).

**Understanding checkpoint:** the framework works by progressively narrowing — "what do I want to show" gets you to one of the four top categories, then follow-up questions (how many variables, how many periods, static or changing) narrow it down to one specific chart. The mistake to avoid is picking a chart by habit (always defaulting to a line chart) instead of running the actual question through this funnel first.

### Where this connects to my pipeline
- **Distribution** — histograms are exactly what I'd reach for to sanity-check return distributions per regime before trusting a GARCH fit (fat tails should be visually obvious before any statistic confirms it).
- **Relationship** — scatter plots are the natural check for the XAUUSD/DXY/XAGUSD correlation and cointegration relationships I've already been treating analytically; seeing it visually first is a good habit before formalizing it statistically.
- **Comparison** — bar charts across sessions/regimes for something like average ATR by session, which is a natural companion to the cross-tabulation work already in the pipeline.

---



### 2026-09-10 — First hands-on pass in Google Sheets

**Tool/concept applied:**
- `=QUARTILE()` to find Q1/median/Q3 on a real column of data.
- `=STDEV.S()` specifically (not the generic `=STDEV()`) — confirms the sample-vs-population distinction above isn't just theoretical; Sheets surfaces it directly as a separate function name.

**Dataset used:**
- certified organic livestock

**What the result showed:**
- a negatively skewed distribution

**Where I got it wrong the first time:**
- well the normal distribution formula

**Next:**
- applying them on historical market data 

## Open Questions

- For fat-tailed return data, does IQR or standard deviation give a more stable "typical spread" estimate session-to-session — worth testing empirically rather than assuming.
- Is there a visualization category on the reference card that maps well to *regime* data specifically (categorical + time-varying), or does that need to be built as a combination of two categories?