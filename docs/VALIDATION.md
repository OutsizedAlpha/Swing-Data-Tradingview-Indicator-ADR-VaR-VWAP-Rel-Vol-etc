# Validation and release acceptance

**Status:** Imported v6.3 research source. The user reported the v6.2 version running in native TradingView after scope/declaration repairs. The v6.3 change consists predominantly of Custom-default table selection, row-control availability and tooltips. **A native TradingView compile of this exact v6.3 source has not been independently observed here.**

**No claims of automated end-to-end signal validation, feed parity, cross-venue numerical accuracy or investment performance are made.**

## Native Pine acceptance checklist

These are procedures to run **inside TradingView**, not tests already certified as passing.

- [ ] Paste the complete `OutsizedAlpha_Swing_Data_Reviewed_v6_3.pine` source and **compile**. Capture exact errors if any.
- [ ] Check 1D US equity with a long history, standard candles, regular session; inspect Market Cap, Float %, Float Turnover, ADR%, ATR%, VaR, VaR 5%, ES 5%, RVOL@Time and VWAP%.
- [ ] Ensure **Table Preset → Custom** makes each corresponding Display tick box change only its own row; no blank gaps.
- [ ] Check **Compact** yields its fixed 15-row content and greys out row controls. Check **Full Research** yields all 39 rows and disables individual controls.
- [ ] Confirm all **nine table positions** and five text sizes; note that 39 large-font rows can exceed the chart height.
- [ ] Change table colours and alignment; confirm no unintended signal changes.
- [ ] Compare the same symbol's daily table with a 60-minute chart during regular trading. On the 60-minute chart, metrics intended as the **daily mirror** should use daily sessions, not one-hour sessions.
- [ ] Check 2D, W and M charts: risk sample periods and regular-session VWAP anchors should follow the chart period, not the daily clock.
- [ ] Compare a stock with missing float data, an ETF and a newly listed security. Unavailable data should show **N/A/—**, not fictitious 0% float or a spurious RS percentile.
- [ ] Check **Float %** using a trustworthy contemporaneous share-count report and examine any disagreement.
- [ ] Check the selected historical VaR and 5% VaR against an **independently calculated** sample of prior completed returns; confirm the current candle is not included.
- [ ] Independently recompute the worst `ceil(0.05 × n)` prior returns to reproduce the displayed ES 5% approximation.
- [ ] Run RVOL@Time intraday and after the close; inspect historical sample `n`, rejected sessions and whether a full-day value is being compared with a same-time historical value.
- [ ] Check 15-minute opening suppression and daily projected share/dollar volume against the stated historical completion fractions.
- [ ] Check Distance from VWAP against the chart's **regular-hours, HLC3, one-minute, session-anchored VWAP**; investigate source-feed discrepancies rather than forcing equality.
- [ ] Check Days to Liquidate shows **Enter position shares** at zero and recomputes when a realistic share position and ADV participation limit are entered.
- [ ] Check that missing `request.seed()` calibration returns **N/A** rather than an assumed RS 99.

## Boundary cases

Test an IPO with less history than the configured lookback, a thinly traded symbol that may lack one-minute data near the open/close, a holiday-shortened week, a chart with long missing historical intervals, and a current period that is still developing.

Check the case of zero high−low range; the indicator should show unavailable efficiency/position metrics rather than divide by zero. Check negligible float turnover before interpreting the price-response ratio.

## Future automation

Automated tests would benefit from an independently reproducible OHLCV dataset with matching exchange session/timestamp and financial-float snapshots. A Python fixture for VaR/ES maths is insufficient to verify `request.security_lower_tf`, nested request context, or live TradingView bar-finalisation behaviour.

**Do not mark a check as passed until it has actually been observed.** Log symbol, exchange, session type, timestamp, timeframe, TradingView subscription/data source, settings and expected versus observed output in each issue.
