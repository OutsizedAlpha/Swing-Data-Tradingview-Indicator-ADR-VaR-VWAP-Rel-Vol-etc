# OutsizedAlpha Swing Data — ADR · VaR · VWAP · Relative Volume

**A configurable, multi-timeframe TradingView market dashboard for price ranges, risk, volume participation, free float, VWAP and liquidity. Written in Pine Script® v6.**

This public repository contains the OutsizedAlpha-expanded derivative of **[jfsrev's original, open-source Swing Data indicator](https://www.tradingview.com/script/uloAa2EI-Swing-Data-ADR-RVol-PVol-Float-Avg-Vol/)**. The upstream author deserves primary credit for the initial dashboard concept and code. OutsizedAlpha's subsequent version includes redesigned risk and volume calculations, several new diagnostics, and a compact/custom/full table system.

[**Download Pine source**](./OutsizedAlpha_Swing_Data_v6_6_ADR_Styled_Table.pine) · [**Methodology**](./docs/METHODOLOGY.md) · [**Validation checklist**](./docs/VALIDATION.md) · [**Contributors and attribution**](./ATTRIBUTION.md) · [**Changelog**](./CHANGELOG.md) · [**MPL-2.0 licence**](./LICENSE)

> **Research tool, not order flow or investment advice.** Some measures are descriptive proxies, not evidence of institutional buying or selling. Inputs, data coverage, feed conventions, chart timeframe, and available intrabars all affect results. The source has not been independently certified against the native TradingView compiler and live market feeds for every timeframe.

## Example chart — SAP SE

<img width="3023" height="1878" alt="image" src="https://github.com/user-attachments/assets/7cf1e4e7-8cd1-45f1-a7cc-b4bcfbb3044f" />

*SAP SE (NYSE), daily chart captured 21 October 2026. The Swing Data dashboard is shown at the lower left of the SAP price pane. The ADR Extension table, other overlays and additional chart panels are separate indicators. Market readings are illustrative and depend on data feed and settings.*

## Getting started

1. Open the [complete Pine Script source](./OutsizedAlpha_Swing_Data_v6_6_ADR_Styled_Table.pine) and copy all of it.
2. In [TradingView](https://www.tradingview.com/), open a chart and select **Pine Editor → Create new indicator**.
3. Replace the editor contents, save, and select **Add to chart**.
4. Use **Settings → Inputs → Table Preset → Custom** (the default). Under **Display**, tick precisely the metrics you want.
5. Select your **Table Position** from all nine chart anchors, and **Table Text Size** from Tiny, Small, Normal, Large or Extra Large.
6. Check the metric tooltips for actual sample sizes, data-source assumptions and eligibility; a dash means a reading is unavailable, not necessarily zero.

**Version in this repository:** v6.6 (source build `OUTSIZEDALPHA-SWING-DATA-v3.6`). It contains **40 selectable table metrics**. The original chart overlay has no trade execution or position management.

## Table behaviour — important

| Preset | What is shown | Individual Display tick boxes |
| --- | --- | --- |
| **Custom (default)** | Exactly the rows you enable | **Active** |
| **Compact** | Fixed 15-row overview modelled on the original dashboard | Disabled while selected |
| **Full Research** | All 40 metric rows | Disabled while selected |

**To show a newly enabled metric, select Custom.** A previously saved TradingView chart instance may retain Compact from an older version, even though v6.6 defaults to Custom. Full Research may exceed the available chart height; choose Small/Tiny text or a selective Custom layout.

Available anchor positions: **Top Left / Top Centre / Top Right; Middle Left / Middle Centre / Middle Right; Bottom Left / Bottom Centre / Bottom Right.** These are Pine table anchors, not freely draggable table coordinates.

The panel also supports left/right text alignment, background and text colours, and an optional empty row at the top to avoid the TradingView pane buttons. The current implementation uses a two-column metric/value table.

## ADR Extension-inspired table styling (v6.6)

The Swing Data table now matches the visual language of the [OutsizedAlpha ADR / MA Extension](https://github.com/OutsizedAlpha/dynamic-standard-deviation-based-multiple-extension-from-moving-average-Tradingview-Indicator). The **calculation engine and the 40 metric switches are unchanged**.

| Element | Default | Effect |
| --- | --- | --- |
| **Blue header** | `#3F589D` | White **OA SWING DATA** title on the left; selected chart timeframe and table preset on the right |
| **Slate-grey body** | `#4E525C` | Opaque background for every metric and value cell, rather than the old transparent fill |
| **Default text** | `#FFFFFF` | Bold white rows for standard metrics |
| **Positive highlight** | `#3FE28C` | Green when the existing metric conditions flag a positive/high reading |
| **Negative highlight** | `#FF686D` | Red when the existing metric conditions flag a negative/low or invalid reading |
| **Warning highlight** | `#FFD36B` | Yellow when the existing conditional warning is triggered |
| **Outer border** | `#D0D1D5` | Thin light-grey frame |

The colour palette can be customised under **Settings → Inputs → Table**. These are **presentation colours only**; they do not change the thresholds governing RVOL, VWAP, directional efficiency, VaR/ES or float reporting.

**Table Text Size now defaults to Small**, keeping the 40-row dashboard usable; Tiny, Normal, Large and Extra Large remain available. **Top Spacer Above Table defaults to Off** so the blue header sits flush at the chosen table anchor. Enable it only when chart controls overlap the header. Existing TradingView chart instances may retain previously saved settings: review the **Table** inputs after replacing the script.

## What the indicator measures

| Group | Metrics | Key interpretation |
| --- | --- | --- |
| **Capital structure** | Market Cap, Shares Float, Float %, Float Turnover | Current equity market data and gross shares traded relative to free float; not unique ownership transfers |
| **Participation** | Participation Intensity Percentile, Relative Volume, RVOL at Time | Unusual trading relative to comparable historical activity, including same-time cumulative or per-block comparisons |
| **Price ranges** | ADR%, optional comparable range%, ATR%, ATR, Day Range Position, Low-of-Period and 52-week distances | Typical ranges, current within-period position and distance to historic extremes |
| **Downside risk** | Configurable empirical VaR, 5% VaR, 5% Expected Shortfall, optional legacy VaR − 3σ | Historical lower-tail descriptions from completed returns |
| **VWAP** | Distance from VWAP | **Percentage price deviation only**; positive means above the relevant anchored VWAP, negative means below |
| **Price response** | Signed Directional Efficiency, Price Response / 1% Float | Candle-body/range proportion and price displacement per gross turnover; **not** actual transaction-level market impact |
| **Activity estimates** | Projected Share Volume, Projected $ Volume, Volume/$ Buzz, Economic Throughput | Historically calibrated volume/notional estimates, dependent on intraday data quality |
| **Liquidity** | Prior daily ADV, 1% ADV Shares, optional Days to Liquidate | Rough shares-per-session capacity assumption, not a market-impact forecast |
| **Comparisons and diagnostics** | Up/Down Candle Volume, approximate RS percentile or relative ratio, Source/Samples, Sector, Industry | Price-strength proxies and explicit data-quality reporting |

Definitions and exact formulas for the most important fields are in [docs/METHODOLOGY.md](./docs/METHODOLOGY.md). Some rows are only meaningful for instruments with equity-specific share and float data.

### Three numbers that are easy to misread

**Directional Efficiency**

```text
100 × (current price − period open) / (period high − period low)
```

It runs approximately from **−100% to +100%**. A reading of +60% means the price's net rise from the open equals 60% of its high–low range; it does **not** mean the stock gained 60%, that the path was smooth, or that institutions accumulated shares.

**Price Response / 1% Float**

```text
selected price return (%) / float turnover (%)
```

A 2% return with 0.5% float turnover produces **4.0 percentage points of return per 1% float turnover**. This is a descriptive ratio, not causal price-impact estimation. Small turnover makes this ratio unstable.

**Days to Liquidate**

```text
position shares / (prior-completed daily ADV × allowed ADV participation fraction)
```

Enter **Parameters → Optional portfolio position (shares)** and set the participation limit (default 5%). A zero share position intentionally displays **Enter position shares**. The result is a trading-session equivalent, ignoring market impact, volume shocks and execution restrictions.

## Historical samples and timeframe conventions

- **Daily chart (1D):** same-time calculations use regular-session intrabars and historical completed daily sessions.
- **Intraday charts below 1D:** the table is designed to mirror the corresponding **daily** calculations rather than treat intraday candles as separate daily observations.
- **2D+, weekly and monthly charts:** the higher-period engine compares corresponding chart-period and session-progress coordinates, rather than treating weekends as traded time.
- **Sampling is adaptive:** requested lookbacks represent target observation counts; the file uses a bounded historical scan (up to 3,000 chart bars) and only publishes advanced readings above defined data-quality/sample thresholds.
- **TradingView data limits still apply:** source-resolution selection, feed permissions, instrument sessions and intrabar availability can lead to smaller observed `n`.
- **Historical float:** the reported annual float series is only an approximation to historical float. When selected, a clearly labelled **[proxy]** ranks same-time volume, *not* historical float turnover.
- **Risk samples:** completed-return calculations exclude the current chart-period return. Default tail window is up to 252 completed periods; the daily minimum for publication is 126 prior completed returns. A 1% tail at 252 returns remains statistically thin.
- **Pine resource use:** multiple requested datasets, lower-timeframe arrays and extended lookbacks may be expensive. Reducing intraday resolution, chart history or selected lookbacks can help when data retrieval is constrained.

### VWAP: percentage distance only

The specified value is deliberately simple:

```text
100 × (current price / native regular-session VWAP − 1)
```

The one-minute **HLC3** anchored VWAP is an estimate from TradingView's available regular-session bar feed. The higher-timeframe engine anchors to the applicable chart period. **No ADR/ATR-based normalisation is applied to the VWAP distance.**

This reading alone does not quantify how long price remained above or below VWAP.

## Risk estimates

**VaR 1%** (with the selected lower-tail percentile at its default) is the empirical 1st-percentile signed return quantile. **VaR 5%** is the separate 5th-percentile cutoff.

**Expected Shortfall 1%** (**on by default, including Compact**) is the **arithmetic mean, not the median**, of the worst `ceil(0.01 × n)` previously completed returns from the same sample. At **252 returns** it averages the worst **three**. **Expected Shortfall 5%** (**optional, enabled separately in Custom**) averages the worst `ceil(0.05 × n)`; at 252 observations it uses the worst **13**. Both return negative percentages for negative tail returns and describe historical severity, not a maximum possible loss.

ES1 and ES5 are calculated across the **1D, below-1D daily-mirror, and native higher-timeframe** paths. Version 6.4 corrected a Pine history-buffer issue so that the ES helper is evaluated on every bar; v6.5 preserves that repair and adds ES1. The method uses discrete `ceil(alpha × n)` tail counts and is an approximation to fractionally weighted Expected Shortfall. Especially at 1%, the sample is sparse and the estimate is volatile.

The selectable legacy **VaR − 3σ** remains a custom severity heuristic, **not** a separately calibrated VaR confidence level or Expected Shortfall.

These figures are empirical descriptions, not price targets, guaranteed worst losses or validated forward-risk forecasts. A missing or incomplete history can display `-`.

## Relative strength and float-data caveats

- The daily **RS percentile [est.]** uses an external `request.seed()` calibration dataset. It publishes a rating only if all seven expected ordered calibration points exist; otherwise, **N/A**. It is **not an official IBD rating**.
- Above 1D, the RS display is a weighted performance **ratio vs SPX**, with 100 denoting matched weighted relative performance; it is not a 1–99 percentile.
- **Float %** is computed against matching outstanding-share counts and is suppressed if those counts are inconsistent. Float coverage may vary by venue and symbol.
- **Gross Float Turnover** can legitimately exceed 100% on very active securities because the same shares may trade repeatedly.

## Testing and reproducibility

The source is published as the completed **v6.3** snapshot, with only a comment-only MPL/attribution header added for the repository. **No executable formulas, engine fields, alerts or settings were modified in this publication step.**

The user previously reported the v6.2 engine running in TradingView; v6.3 added Custom-default table controls and explanatory tooltips. That does **not** establish native compilation or parity across all markets/timeframes.

Before relying on live values, complete the [manual acceptance checklist](./docs/VALIDATION.md): compile in native TradingView, check 1D against intraday mirror, compare higher-timeframe behaviour, test table presets/toggles, check missing-float instruments and inspect the tooltip `n`/source diagnostics. This repository does not contain a CI pipeline or a verified native TradingView integration test.

Bug reports should include symbol, exchange, session, timeframe, settings, the complete Pine error and, for numeric discrepancies, a small OHLCV reproduction. See [CONTRIBUTING.md](./CONTRIBUTING.md).

## Attribution and intellectual property

**Primary upstream source:** [Swing Data - ADR% / RVol / PVol / Float % / Avg $ Vol](https://www.tradingview.com/script/uloAa2EI-Swing-Data-ADR-RVol-PVol-Float-Avg-Vol/) by **jfsrev (Jeff)**, originally published **24 April 2023**. This repository is a **derivative**, not a claim that OutsizedAlpha originated the full indicator.

**Additional public credit:** the source history identifies [Bob-Lablaw's Projected Volume](https://www.tradingview.com/script/tFe9aLsd-Projected-Volume/) as a source of earlier projected-volume logic. The upstream jfsrev release notes thank **@DumbleDax** and **@lacedtrades** in connection with earlier market-cap fixes. Their specific contributions should not be conflated with OutsizedAlpha's later enhancements.

Please read [ATTRIBUTION.md](./ATTRIBUTION.md) for the fuller provenance and licence notes. All contributors retain their own rights. Neither TradingView nor jfsrev has endorsed this repository.

## Licence

Distributed under **[Mozilla Public License 2.0](./LICENSE)**. Open-source TradingView scripts default to MPL-2.0 unless the creator specifies another licence; this project retains that default for its derivative source and supplies the full licence text. The licence's source-level attribution and notice obligations apply. See [TradingView open-source reuse rules](https://www.tradingview.com/support/solutions/43000590599-script-publishing-rules/) separately if republishing on TradingView itself.

**For educational chart research only. No performance or profit guarantee.**
