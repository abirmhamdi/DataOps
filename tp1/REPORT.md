# Lab 1 - Report

## Step 0

**Q1.** requirements.txt lists the libraries the project needs, so anyone on any machine can rebuild the same environment with one command (pip install -r requirements.txt). This makes the project reproducible, and nobody has to remember what to install by hand.

## Step 1

**Q2.** The report shows 8 lines instead of 5. The same country is written in different ways, and the script treats each spelling as a different country: "Tunisia", "tunisia" (order 1006), " Tunisia" with a leading space (order 1023), "Morocco" and "MOROCCO" (order 1016), plus France, Italy and Germany.

**Q3.**
- Order 1004: negative quantity (-2). Dimension: Validity.
- Order 1008: customer_id is missing. Dimension: Completeness.
- Order 1010: appears twice, so it is counted twice. Dimension: Uniqueness.
- Order 1013: the date 2026-13-45 does not exist. Dimension: Validity.
- Order 1019: unit price of 4500 for a keyboard that costs 45. Dimension: Accuracy.
- Orders 1006, 1016, 1023: the country is written differently (tunisia, MOROCCO, " Tunisia"). Dimension: Consistency.

**Q4.** The total is too high. Most problems add revenue that should not be there: the duplicate order 1010 adds 449, and the price error on order 1019 adds about 4,455 too much. Only the negative quantity (order 1004) lowers the total, by about 40. The errors do not cancel out, so the result is far too high. The script printed no error and no warning, so this is a silent error.

**Q5.** Splitting the script into functions makes the code easier to read and maintain. It also allows us to test and modify each pipeline step independently.

**Q6.** The correct total is 7655.90. The old script reported 12771.10, which is an overstatement of approximately 66.82% compared with the correct total.

**Q7.** Order 1019 caused the largest part of the error because its unit price was 4500 instead of 45. It was caught by Rule 4, which checks that the unit price is between 0.01 and 1000.

**Q8.** Saving rejected rows allows us to investigate the errors, understand why the rows were rejected and correct the source data if necessary. It also keeps a record of rejected data instead of losing it.