# 📊 My Data Analytics Learning Log — Google Sheets Basics

> **Why this file exists:** I'm learning data science, starting with spreadsheets. This is a journal of what I've learned so far — written so that a total beginner (like I was a few days ago) can follow it step by step. I'm committing this to GitHub as part of documenting my learning journey.

---

## 🧭 Part 1: Navigation & Workspace Shortcuts

### Moving the cursor

| Shortcut | What it does |
|---|---|
| `Ctrl + Arrow key` | Jumps to the edge of a block of data in that direction |
| `Ctrl + Home` | Jumps straight to cell A1 (top-left) |
| `Ctrl + End` | Jumps to the last cell that has data |
| `Ctrl + Page Up` / `Ctrl + Page Down` | Switches between sheet tabs |
| `Enter` | Moves down one cell after typing |
| `Shift + Enter` | Moves up one cell after typing |
| `Tab` | Moves right one cell |
| `Shift + Tab` | Moves left one cell |
| `Ctrl + G` then type cell ref | Jumps directly to a named cell (e.g. `B45`) |

### Selecting data

| Shortcut | What it does |
|---|---|
| `Ctrl + A` | Selects all data in the sheet |
| `Ctrl + Space` | Selects the entire column |
| `Shift + Space` | Selects the entire row |
| `Ctrl + Shift + Arrow key` | Extends your selection to the edge of the data |
| `Shift + Click` | Selects a range between two points |
| `Ctrl + Click` | Adds a separate, non-adjacent cell/range to your selection |

### Editing & formatting

| Shortcut | What it does |
|---|---|
| `F2` or double-click a cell | Edit a cell directly |
| `Alt + Enter` | Adds a line break *inside* a cell (without moving to the next one) |
| `Ctrl + \` | Clears formatting on selected cells |
| `Ctrl + B` / `Ctrl + I` / `Ctrl + U` | Bold / Italic / Underline |
| `Ctrl + Shift + 7` | Adds borders to selection |
| `Ctrl + Shift + 1` | Formats a number as a decimal |
| `Ctrl + Shift + 4` | Formats a number as currency |
| `Ctrl + Shift + 5` | Formats a number as a percentage |
| `Alt + Shift + 5` (or right-click) | Strikethrough text |

### Rows, columns & sheets

| Shortcut | What it does |
|---|---|
| `Ctrl + Alt + =` | Insert row/column (opens menu) |
| `Ctrl + Alt + -` | Delete row/column |
| `Alt + Shift + F5` (or right-click tab) | Insert a new sheet |
| `Ctrl + Shift + K` | Insert a link |
| `Ctrl + Alt + E, then E` | Delete the current row |

### Finding, replacing & general actions

| Shortcut | What it does |
|---|---|
| `Ctrl + F` | Find |
| `Ctrl + H` | Find and replace |
| `Ctrl + Z` / `Ctrl + Y` | Undo / Redo |
| `Ctrl + C` / `Ctrl + X` / `Ctrl + V` | Copy / Cut / Paste |
| `Ctrl + Shift + V` | Paste values only (no formatting/formulas) |
| `Ctrl + S` | Save (Sheets auto-saves, but this forces it / shows status) |
| `Ctrl + /` | Opens the shortcut cheat-sheet inside Sheets itself |
| `Alt + /` or `=` | Starts a formula in a cell |

**Beginner tip:** You don't need to memorize all of these on day one. Start with `Ctrl + Arrow`, `Ctrl + Space`/`Shift + Space`, `Ctrl + Z`, and `Ctrl + F`. The rest will stick as you use the sheet more.

---

## 🧮 Part 2: Formulas — Making the Data Useful

Grouped by *what problem they solve*, not just alphabetically — that's usually more helpful.

### A. Summary / Aggregation formulas ("tell me the big picture")

| Formula | Syntax | What it does | Example |
|---|---|---|---|
| `SUM` | `=SUM(range)` | Adds up all numbers in a range | `=SUM(A1:A10)` |
| `AVERAGE` | `=AVERAGE(range)` | Finds the mean value | `=AVERAGE(B2:B20)` |
| `MEDIAN` | `=MEDIAN(range)` | Finds the middle value when sorted | `=MEDIAN(B2:B20)` |
| `MODE` | `=MODE(range)` | Finds the most frequently occurring value | `=MODE(B2:B20)` |
| `COUNT` | `=COUNT(range)` | Counts cells containing **numbers** | `=COUNT(C2:C50)` |
| `COUNTA` | `=COUNTA(range)` | Counts **non-empty** cells (numbers or text) | `=COUNTA(C2:C50)` |
| `COUNTBLANK` | `=COUNTBLANK(range)` | Counts empty cells | `=COUNTBLANK(C2:C50)` |
| `MAX` / `MIN` | `=MAX(range)` / `=MIN(range)` | Highest / lowest value | `=MAX(D2:D30)` |
| `SUMIF` | `=SUMIF(range, criterion, [sum_range])` | Adds values that meet one condition | `=SUMIF(A2:A10,"Fruit",B2:B10)` |
| `SUMIFS` | `=SUMIFS(sum_range, range1, crit1, ...)` | Adds values that meet **multiple** conditions | `=SUMIFS(C2:C10,A2:A10,"Fruit",B2:B10,">5")` |
| `COUNTIF` | `=COUNTIF(range, criterion)` | Counts cells matching one condition | `=COUNTIF(A2:A10,"Fruit")` |
| `COUNTIFS` | `=COUNTIFS(range1, crit1, ...)` | Counts cells matching **multiple** conditions | `=COUNTIFS(A2:A10,"Fruit",B2:B10,">5")` |
| `AVERAGEIF` | `=AVERAGEIF(range, criterion, [avg_range])` | Averages values matching a condition | `=AVERAGEIF(A2:A10,"Fruit",B2:B10)` |
  `FILTER` | = `FILTER(reange , condition)`  -filters the data as specified 
👉 These are usually the *first* things you calculate when exploring a new dataset.

---

### B. Rounding formulas ("clean up messy decimals")

This is the part that confused me most as a beginner.

| Formula | Syntax | What it does |
|---|---|---|
| `ROUND` | `=ROUND(number, num_digits)` | Rounds to the nearest value (standard rounding — up or down depending on the digit) |
| `ROUNDUP` | `=ROUNDUP(number, num_digits)` | Always rounds **up**, regardless of the digit |
| `ROUNDDOWN` | `=ROUNDDOWN(number, num_digits)` | Always rounds **down** (truncates), regardless of the digit |
| `CEILING` | `=CEILING(number, significance)` | Rounds **up** to the nearest multiple of `significance` |
| `CEILING.PRECISE` | `=CEILING.PRECISE(number, significance)` | Same as CEILING but always rounds up toward +∞, even with negative numbers |
| `CEILING.MATH` | `=CEILING.MATH(number, [significance], [mode])` | Flexible version of CEILING with an optional "mode" for negative-number behavior |
| `FLOOR` | `=FLOOR(number, significance)` | Rounds **down** to the nearest multiple of `significance` (the opposite of CEILING) |
| `FLOOR.PRECISE` | `=FLOOR.PRECISE(number, significance)` | Same as FLOOR but consistent with negative numbers |
| `FLOOR.MATH` | `=FLOOR.MATH(number, [significance], [mode])` | Flexible version of FLOOR |
| `MROUND` | `=MROUND(number, multiple)` | Rounds to the **nearest** multiple (not always up or down) |
| `TRUNC` | `=TRUNC(number, [num_digits])` | Chops off decimals without rounding at all |
| `INT` | `=INT(number)` | Rounds **down** to the nearest whole number |
| `MOD` | `=MOD(number, divisor)` | Gives the **remainder** after division |

#### Worked examples (the ones I specifically tested)

```
=CEILING(7, 2)            → 8    (rounds up to next multiple of 2)
=CEILING.PRECISE(-3, 2)   → -2   (always rounds toward +∞, unlike plain CEILING with negatives)
=ROUNDUP(4.567, 2)        → 4.57 (rounds up to 2 decimal places)
=CEILING.MATH(4, 1)       → 5    (rounds up to next whole number)
=MOD(10, 3)               → 1    (10 ÷ 3 = 3 remainder 1)
```

⚠️ **Beginner traps I ran into:**
1. In `=CEILING.MATH(D2, 2-1)`, Sheets calculates `2-1` **before** running the formula — so it's really `=CEILING.MATH(D2, 1)`. Always simplify the math inside the parentheses by hand if unsure.
2. `ROUNDUP`'s second argument is a **count of decimal places** (a whole number like 0, 1, 2) — not a decimal like `0.01`. Writing `=ROUNDUP(D1, 0.01)` gets truncated to `=ROUNDUP(D1, 0)`. For 2 decimal places (money), use `=ROUNDUP(D1, 2)`.
3. `CEILING`/`FLOOR` round to a **multiple** (nearest 2, nearest 5). `ROUND`/`ROUNDUP`/`ROUNDDOWN` round to a **number of decimal places**. Mixing these two ideas up is the most common mistake.

---

### C. Logical formulas ("make decisions based on data")

| Formula | Syntax | What it does |
|---|---|---|
| `IF` | `=IF(condition, value_if_true, value_if_false)` | Returns one value if a condition is true, another if false |
| `IFS` | `=IFS(cond1, val1, cond2, val2, ...)` | Checks multiple conditions in order, like several IFs stacked |
| `AND` | `=AND(condition1, condition2, ...)` | TRUE only if **all** conditions are true |
| `OR` | `=OR(condition1, condition2, ...)` | TRUE if **any** condition is true |
| `NOT` | `=NOT(condition)` | Flips TRUE to FALSE and vice versa |
| `IFERROR` | `=IFERROR(value, value_if_error)` | Shows a fallback value instead of an error message |

Example: `=IF(B2>50,"Pass","Fail")` — checks a score and labels it.

---

### D. Lookup & reference formulas ("find data elsewhere in the sheet")

| Formula | Syntax | What it does |
|---|---|---|
| `VLOOKUP` | `=VLOOKUP(search_key, range, index, [is_sorted])` | Looks up a value in the first column of a range, returns a value from another column |
| `HLOOKUP` | `=HLOOKUP(search_key, range, index, [is_sorted])` | Same as VLOOKUP but searches across a row instead of a column |
| `INDEX` | `=INDEX(range, row, column)` | Returns the value at a specific row/column position in a range |
| `MATCH` | `=MATCH(search_key, range, [type])` | Returns the **position** of a value within a range |
| `INDEX + MATCH` | `=INDEX(range, MATCH(search_key, lookup_range, 0))` | A more flexible combo often used instead of VLOOKUP |
| `XLOOKUP` | `=XLOOKUP(search_key, lookup_range, result_range)` | Newer, more flexible lookup (searches in any direction) |

---

### E. Text formulas ("clean and reshape words, not numbers")

| Formula | Syntax | What it does |
|---|---|---|
| `CONCATENATE` | `=CONCATENATE(text1, text2, ...)` | Joins text pieces together |
| `&` operator | `=A1&" "&B1` | Shortcut way to join text without CONCATENATE |
| `LEFT` / `RIGHT` | `=LEFT(text, num_chars)` | Grabs characters from the start / end of a text string |
| `MID` | `=MID(text, start, num_chars)` | Grabs characters from the middle of a text string |
| `LEN` | `=LEN(text)` | Counts how many characters are in a text string |
| `TRIM` | `=TRIM(text)` | Removes extra spaces |
| `UPPER` / `LOWER` / `PROPER` | `=UPPER(text)` | Changes text case (ALL CAPS / lowercase / Title Case) |
| `SPLIT` | `=SPLIT(text, delimiter)` | Breaks text into separate cells using a delimiter (e.g. comma) |
| `SUBSTITUTE` | `=SUBSTITUTE(text, search, replace)` | Replaces specific text within a string |

---

### F. Date & time formulas ("track when things happened")

| Formula | Syntax | What it does |
|---|---|---|
| `TODAY` | `=TODAY()` | Returns today's date |
| `NOW` | `=NOW()` | Returns today's date **and** current time |
| `DATE` | `=DATE(year, month, day)` | Builds a date from separate numbers |
| `DATEDIF` | `=DATEDIF(start_date, end_date, unit)` | Calculates the difference between two dates |
| `YEAR` / `MONTH` / `DAY` | `=YEAR(date)` | Pulls out just the year / month / day from a date |
| `WEEKDAY` | `=WEEKDAY(date)` | Returns which day of the week a date falls on |
| `NETWORKDAYS` | `=NETWORKDAYS(start_date, end_date)` | Counts working days between two dates (excludes weekends) |

---

### G. Handy quick-reference cheat sheet

| I want to... | Use this |
|---|---|
| Add up numbers | `SUM` |
| Add up numbers meeting a condition | `SUMIF` / `SUMIFS` |
| Get the average | `AVERAGE` |
| Count how many entries there are | `COUNTA` |
| Count entries meeting a condition | `COUNTIF` / `COUNTIFS` |
| Round up to a whole number | `ROUNDUP(number, 0)` or `CEILING.MATH(number, 1)` |
| Round up to 2 decimal places (money) | `ROUNDUP(number, 2)` |
| Round up to a specific step (e.g. nearest 5) | `CEILING(number, 5)` |
| Round up safely with negative numbers | `CEILING.PRECISE(number, significance)` |
| Round down instead of up | `FLOOR` / `ROUNDDOWN` |
| Get a remainder / check even-odd | `MOD(number, 2)` |
| Look up a value from another table | `VLOOKUP` or `INDEX` + `MATCH` |
| Make a decision based on a condition | `IF` |
| Join text together | `CONCATENATE` or `&` |
| Work with dates | `TODAY`, `DATE`, `DATEDIF` |

---

📈 Part 3: Descriptive Statistics — Making Sense of a Dataset

After formulas and navigation, the next layer is descriptive statistics — using a handful of numbers to summarize an entire dataset. Two big families here: measures of central tendency (what's "typical") and measures of spread (how scattered the data is).

A. Measures of central tendency

These try to answer: "if I had to describe this dataset with just one number, what would it be?"

Measure	What it tells you	Formula	Best used when
Mean	The arithmetic average — add everything up, divide by how many values there are	=AVERAGE(range)	The data has no major outliers (the mean is easily skewed by extreme values)
Median	The middle value once the data is sorted from smallest to largest	=MEDIAN(range)	The data has outliers — median ignores extreme values better than mean
Mode	The value that shows up most often	=MODE(range)	The data is categorical (grouped into a fixed number of categories) rather than continuous

Worked example (mean): For {1, 3, 5, 8, 10, 13, 16}, add them up (56) and divide by the count (7) → mean = 8.

Worked example (median): Same dataset, already sorted, 7 values → the 4th value (the middle one) is the median → 8.

Worked example (mode): For {5, 1, 5, 8, 5, 1, 5}, the number 5 shows up most → mode = 5.

👉 Beginner insight: Mean, median, and mode can all be different numbers for the same dataset — that's normal. Which one you trust depends on whether your data has outliers or is categorical.

B. Measures of spread

These try to answer: "how scattered is this data around that typical value?" A "typical value" alone can be misleading without knowing how spread out the rest of the data is.

Measure	What it tells you	Formula
Range	The gap between the largest and smallest value	=MAX(range) - MIN(range)
Quartiles	Splits sorted data into four equal quarters (Q1, Q2/median, Q3)	=QUARTILE(data, quartile_number)
Interquartile Range (IQR)	The spread of just the middle 50% of the data (between Q1 and Q3)	=QUARTILE(data, 3) - QUARTILE(data, 1)
Variance	The average of the squared differences between each data point and the mean — measures overall variability	=VAR(range)
Standard Deviation	The square root of variance — variability expressed in the same units as the original data (easier to interpret than variance)	=STDEV(range)

Worked example (range): For {1, 3, 5, 8, 10, 13, 16}, range = 16 − 1 = 15.

Worked example (IQR): For {1, 3, 5, 6, 8, 10, 13, 16, 17, 19, 22}, Q3 = 17 and Q1 = 5, so IQR = 17 − 5 = 12. This tells you the middle half of the data spans 12 units.

👉 Beginner insight: Variance is mathematically important but hard to interpret on its own because it's in "squared units." Standard deviation fixes that by converting back to normal units — that's why it's the one most commonly quoted (e.g. "the average was 50 ± 5").

C. Central tendency + spread — quick-reference cheat sheet
I want to...	Use this
Find the typical value (no outliers)	AVERAGE
Find the typical value (has outliers)	MEDIAN
Find the most common category	MODE
See the full spread of the data	MAX - MIN (Range)
See the spread of just the middle 50%	QUARTILE(data,3) - QUARTILE(data,1)
Measure overall variability (technical)	VAR
Measure overall variability (interpretable)

## 📝 Notes to self (and to future readers)

- Spreadsheet formulas calculate the **math inside the parentheses first**, before applying the function — so `2-1` becomes `1` before `CEILING.MATH` even sees it. Simplify by hand first if unsure.
- `CEILING`/`FLOOR`-family formulas round to a **multiple** (nearest 2, nearest 5). `ROUND`-family formulas round to a **number of decimal places**. Don't mix these two ideas up.
- Practicing with small, obvious numbers (like 7, 10, -3) before using real data makes it much easier to trust a formula is doing what you expect.
- `SUMIF`/`COUNTIF`/`AVERAGEIF` and their "S" versions (multiple conditions) come up constantly in real analysis — worth mastering early.

---

*This log will keep growing as I learn more — next up: pivot tables, basic charting, and data validation.*
