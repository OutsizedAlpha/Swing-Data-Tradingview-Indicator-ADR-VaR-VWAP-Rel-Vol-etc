# Methodology and calculation boundaries

This document explains the **v6.6 implementation**, not an independent certification of values from TradingView's realtime feed. The [canonical source](../OutsizedAlpha_Swing_Data_v6_6_ADR_Styled_Table.pine) is authoritative where exact branching and availability rules matter.

> **Presentation-only v6.6 update:** the optional table now uses a blue header, slate-grey cells, white text and configurable green/red/yellow highlights. These appearance options do **not** change any mathematical metric definition in this document.

## Data contexts

| Chart | Engine and interpretive grain |
| --- | --- |
| 1D | Current regular-session daily OHLCV with request for lower-timeframe historical intrabars |
| Below 1D | Daily-mirror engine: statistics expressed as 1D readings, **not recalculated as intraday-period statistics** |
| Above 1D (2D+, W, M) | Chart-period engine; session ordinal and elapsed fraction represent partial completed/developing periods |

Daily regular-hours bounds and historical source availability are validated. Different providers and plan entitlements can return different histories. The current bar can update as volume/price arrive; historical risk thresholds specifically use **prior completed** returns.

Data windows are based on **chart periods** and TradingView's available history, not calendar-date arithmetic.

## Market capitalisation, float and turnover

**Float percentage**

```text
100 × free-float shares / total shares outstanding
```

The table requires both share counts to be positive/consistent; it shows N/A for conflicting denominators rather than displaying values over 100%. Market capitalisation and float shares may come from different financial-reporting vintages; inspect source consistency before making valuation inferences.

**Float turnover**

```text
100 × regular-session shares traded so far / current available free-float shares
```

Turnover is gross volume. The same free-float share can trade several times: turnover may exceed 100% and cannot be interpreted as 100% of holders selling.

**Participation Intensity Percentile**

The current turnover intensity is ranked against historical intensity at the **same elapsed session point**. When annual reported-float history is incomplete, the optional **[V-proxy]** uses a *separate* same-time volume distribution. It does not mix reported-float and volume rankings within one sample. The annual float series is not a publication-date point-in-time ownership record.

Ranks use a midpoint tie adjustment, expressed as a percentile. The default daily publication floor is **63 valid observations**; the table reports actual `n`.

## Price ranges and volatility

| Output | Formula or meaning |
| --- | --- |
| **ADR% — legacy** | `100 × (SMA(high / low, ADR length) − 1)` |
| **Range% / previous close** | `100 × SMA((high−low) / previous close, ADR length)` |
| **ATR%** | `100 × Wilder ATR / current price` (includes overnight gaps) |
| **Day Range Position** | `100 × (current price−period low)/(period high−period low)` |
| **Directional Efficiency** | `100 × (current price−period open)/(period high−period low)` |
| **Low distance** | Price−period-low expressed in ATR units |
| **52W distances** | Percentage distance from 52-week high and low on the relevant weekly source |

ADR% and ATR% are not identical because of lookback, normalisation and gap treatment. **Directional Efficiency** is signed candle-body size relative to its range; it is *not* intraday path smoothness, buy/sell initiator imbalance, or an institutional accumulation indicator.

For zero range, division-based position/efficiency readings are unavailable.

## Historical downside-risk estimates

The signed period return is:

```text
r(t) = 100 × (close(t) / close(t−1) − 1)
```

(or a decimal rather than a percentage if the relevant setting is disabled).

The main risk rows use **completed returns before the current period**:

- **Selected VaR:** interpolated empirical quantile at the selected lower-tail percentage, default **1%**.
- **5% Historical VaR:** separately estimated fifth-percentile return cutoff on the same completed-return sample.
- **Expected Shortfall 1% (default):** arithmetic mean of the worst `ceil(0.01 × n)` completed returns. With 252 valid returns the worst **three** are averaged; with 126, the worst **two**. **Not a median**, and not a calibrated maximum loss.
- **Expected Shortfall 5% (optional):** mean of the worst `ceil(0.05 × n)` completed returns, e.g. the worst **13** when `n=252`.
- **Pine history-buffer consideration:** both ES helpers must run on every bar for their historical parameter buffers to be correctly populated; the tail sorting is performed only when needed. The daily-mirror and higher-timeframe engines provide these same two ES definitions independently.
- **Approximation:** both discrete tail means use `ceil(alpha × n)`; they are not fractionally weighted estimators of a continuous loss distribution.
- **Legacy VaR − 3σ:** custom severity heuristic; no assigned confidence level, not conventional historical ES.

Default maximum is **252 returns**, minimum **126 completed daily returns**. Higher chart periods use separately selected minimums. Actual sample length can be smaller than requested due to data availability. At `n=252`, there are only around 2.52 expected observations within a 1% population tail: the estimate is statistically fragile.

These are **descriptive historical returns**, not maximum possible drawdowns, future price targets, calibrated stress-loss probabilities, or position-level portfolio VaR.

## Relative volume at time

Cumulative mode compares:

```text
Current cumulative session volume
-------------------------------------  = RVOL multiple
Historical mean cumulative volume at matching elapsed session time
```

The default UI can display **Difference (+/−%)** = `100 × (multiple−1)`, **Index** = `100 × multiple`, or a plain **Ratio (x)**.

Per-block mode instead compares the current block with historically matched blocks. A developing source bar may be time-weighted as an approximation. Source cumulative-volume validation, incomplete first-session exclusion, missing-session tests and eligible-sample counts are intended to prevent mixing broken historical denominators with current volume.

The **21-valid-session** default daily minimum applies to the same-time average. Sample lengths can be shorter than the requested target because of intrabar history budget, feed limitations or other rejected sessions.

## Volume projections and activity

For a developing daily session, projected shares are based on a **historical cumulative completion fraction**, not linear extrapolation over elapsed calendar time:

```text
Projected full-session shares ≈ shares traded so far / typical historical share completion fraction
```

Historical same-time session curves are constructed from suitable past sessions. The daily projection is withheld before the configured minimum elapsed minutes (default **15**) and when fewer than 21 valid projection observations support it.

**Projected dollar volume** uses a distinct historical notional-completion estimate, with transaction value approximated from available intrabar price × volume data. A bar-based HLC3 estimate is not consolidated, trade-by-trade notional volume.

**Volume Buzz** reports the projected share-volume deviation relative to prior typical volume; its dollar counterpart uses estimated notional volume. These are seasonal extrapolations, not certain forecasts.

**Economic Throughput** is approximately:

```text
100 × estimated session notional volume / free-float market value
≈ float turnover (%) × VWAP / current price
```

It is strongly correlated with float turnover. It does **not** measure net capital inflows or institutional ownership.

## VWAP and price acceptance

The indicator intentionally reports **percentage distance only**:

```text
Distance from VWAP (%) = 100 × (current price / session VWAP − 1)
```

VWAP is computed using regular-session lower-timeframe HLC3/volume data, generally one-minute bars for the daily measurement. Above 1D, an anchored chart-period variant is used. The source must pass session/time coverage checks; an unavailable or wrong-session snapshot is not silently substituted.

This is **not an ADR/ATR multiple**. A positive reading means price is currently above VWAP, not that price has been persistently above it for any defined number of minutes.

## Directional price response and liquidity

**Price Response / 1% Float**

```text
price return in percentage points / gross float turnover in %
```

A +2% price move with 0.5% gross float turnover produces +4.0 percentage points per 1% turnover. The numerator's start can be the session open or previous close. This measure does not causally attribute price movement to the observed volume.

**Days to Liquidate**

```text
entered position shares / (prior-completed daily ADV × user participation rate)
```

The default allowable participation is 5% of prior daily ADV. It is only a simplified trading-day-equivalent throughput measure, ignoring price impact, market closure, volume variation, limits and execution strategy. The position shares field must be entered to display a figure.

## Relative strength

The daily derived RS score uses weighted stock returns across 63/126/189/252 daily bars compared with corresponding SPX returns. A seed-based external calibration provides an **estimated percentile only if seven valid ordered points are available**. It is not official IBD or an independently calculated cross-sectional rank.

Higher timeframes display an *unranked weighted relative-performance ratio*, typically centred on 100, not a 1–99 rank.

## Sources, confidence and restrictions

- **Financial data:** TradingView financial series and symbol metadata may be missing, updated or inconsistent. Historical annual float is **not** exact publication-date-aware ownership.
- **Intraday feeds:** Sampling bars, opening/closing-auction handling, regular-hours sessions, and subscription intrabar limits can change numerical results.
- **Pine execution:** `request.security`, `request.security_lower_tf`, `request.financial` and `request.seed` have platform-specific contexts and request limits.
- **Sample quality:** Visible `n` shows observations passing the available checks, not their statistical representativeness.
- **Unverified:** The source is not accompanied by a successful native-compiler or cross-feed parity certificate. See [VALIDATION.md](./VALIDATION.md).
