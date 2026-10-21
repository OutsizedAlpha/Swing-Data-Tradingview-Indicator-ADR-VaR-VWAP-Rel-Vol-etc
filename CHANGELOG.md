# Changelog

## v6.3 — initial public repository snapshot (21 October 2026)

- Imported the full OutsizedAlpha Swing Data Pine Script v6.3 source with **comment-only MPL-2.0 and upstream attribution header**.
- Switched to **Custom as the default table preset**, allowing 39 individual Display check boxes to control their respective rows. Compact and Full Research have explicitly disabled overrides; Compact retains a fixed 15-row layout.
- Added clear input tooltips for Directional Efficiency, float-turnover price response, separate 5% VaR, 5% Expected Shortfall, Source/Samples, estimated Days to Liquidate, VWAP percentage distance and range normalisation.
- Retained 9 table anchors, 5 text sizes, dense layout without hidden-row gaps, and existing mathematical calculations from the prior working engine.
- Retained the source's historical-float and same-time-volume proxy distinctions and effective sample-quality reporting.

### Earlier development represented in the imported source, not retroactive commits

**v6.2**
- Repaired scope of intraday-mirror projection counters and restored the required `tfDaily` table-context flag.
- Preserved prior indentation repair near VWAP/directional-efficiency assignments.

**v6.1**
- Corrected the indentation error that blocked the native Pine parser near one VWAP assignment.

**v6.0 extensive review**
- Introduced completed-return empirical VaR, 5% Expected Shortfall and an optional legacy stress heuristic.
- Added share-count reconciliation, historical seasonal volume projections, directional metrics and estimated liquidation days.
- Extended same-time historical scanning, RS calibration validation, and display presets.
- Preserved the percentage-only VWAP distance; no ATR/ADR multiple was added to it.

Development before this GitHub repository took place in separate exchanges. These entries document the imported source history; the repository **does not fabricate earlier Git commits or claim they were fully native-compiler-tested**.

## Planned improvements

Native TradingView validation across 1D/intraday/higher timeframes; reproducible OHLCV/financial-data fixtures; efficiency profiling for large intrabar datasets; targeted corrections when supported by observed discrepancies.
