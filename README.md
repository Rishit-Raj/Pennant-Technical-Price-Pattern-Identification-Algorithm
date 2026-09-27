# Pennant Technical Pattern Identification Algorithm

A Python implementation that automatically detects **Bull Pennant** and **Bear Pennant** chart formations from financial time series data. Two independent detection methods are provided — a **PIP-based** approach and a **trendline regression** approach — along with built-in backtesting analytics to measure post-pattern returns.

---

## What Are Pennant Patterns?

Pennant patterns are short-term continuation formations that appear after a strong, sharp price move (the **pole**), followed by a brief period of converging price consolidation (the **pennant body**), and then a breakout resuming the original trend direction.

Unlike flags, which generally form parallel price channels, pennants are characterized by **converging support and resistance trendlines**.

### Bull Pennant
- Appears after a sharp **upward** move (the pole)
- Price consolidates between **converging support and resistance trendlines**
- Resistance slopes downward while support slopes upward
- Confirmed when price breaks **above** the upper resistance trendline
- Signals continuation of the uptrend

### Bear Pennant
- Appears after a sharp **downward** move (the pole)
- Price consolidates between **converging support and resistance trendlines**
- Resistance slopes downward while support slopes upward
- Confirmed when price breaks **below** the lower support trendline
- Signals continuation of the downtrend

**Key geometric constraints applied in this implementation:**
- Pennant width must be less than 50% of the pole width
- Pennant height must be less than 50–75% of the pole height (method-dependent)
- Support and resistance trendlines must **converge**
- Resistance must have a negative slope
- Support must have a positive slope
- The projected intersection of the trendlines must occur ahead of the pennant body

---

## Project Structure

```text
pennants.py                # Main pattern detection, backtesting, and visualization
perceptually_important.py  # Perceptually Important Points (PIP) extraction algorithm
trendline_automation.py    # Automated trendline fitting via slope optimization
```

---

## Detection Methods

### 1. PIP-Based Detection (`find_pennants_pips`)

Uses **Perceptually Important Points (PIPs)** — a technique that iteratively selects the most structurally significant price points by maximizing perpendicular, vertical, or Euclidean distance from a connecting line.

Five PIPs are extracted from the pennant body and their geometric relationships are validated to confirm or reject each candidate pattern.

The algorithm checks for:

- Alternating local peaks and troughs
- Lower successive highs
- Higher successive lows
- Downward-sloping resistance
- Upward-sloping support
- Convergence of the two boundaries
- Pennant dimensions relative to the preceding pole
- Correct breakout direction

### 2. Trendline Regression Detection (`find_pennants_trendline`)

Fits automated **support and resistance trendlines** to the pennant body using least-squares regression as an initial estimate, then refines the slopes via numerical optimization to ensure the lines are tangent to price extrema.

Unlike flag detection, the fitted trendlines must **converge rather than remain approximately parallel**.

Pattern confirmation is triggered when:

- Support has a positive slope
- Resistance has a negative slope
- The two trendlines converge
- Pennant width and height satisfy the geometric constraints
- Price breaks above resistance for a bull pennant or below support for a bear pennant
