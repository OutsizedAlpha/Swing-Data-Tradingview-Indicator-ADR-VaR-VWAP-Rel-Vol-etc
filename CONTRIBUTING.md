# Contributing

Contributions and reproducible bug reports are welcome, particularly from users who can compare the calculations against TradingView's live feeds and independent OHLCV records.

## What to include in a bug report

- **Symbol, exchange, chart timeframe, regular/extended-hours session, feed and date/time**.
- Indicator source version, Pine Editor compile message (including the full line number), and all relevant input settings.
- Expected result, observed result, supporting numeric evidence or a screenshot.
- For volume metrics: selected intrabar resolution, same-time window, valid sample count `n`, and any data-quality exclusions.
- For float/market-cap discrepancies: share-count source, relevant financial-reporting period and as-of date.
- For risk: the exact historical-return series and whether the latest return was developing.

Never upload private account credentials, position holdings or confidential market data.

## Pull request checklist

1. Preserve the [MPL-2.0](./LICENSE) source-file notice and the credits documented in [ATTRIBUTION.md](./ATTRIBUTION.md); do not remove jfsrev/Bob-Lablaw attribution.
2. Make clear whether the change affects **calculations, source handling, sample-quality criteria, signals, presets or presentation**.
3. Update [docs/METHODOLOGY.md](./docs/METHODOLOGY.md) and [CHANGELOG.md](./CHANGELOG.md) for material changes.
4. Maintain the **percentage-only VWAP distance** unless a future owner explicitly requests a different definition.
5. Avoid look-ahead: downside-risk reference samples must consist of **prior completed returns**.
6. Do not turn missing historical float into made-up values or silently relabel volume ranks as historical-float ranks.
7. In Custom mode, each of the **39 Display toggles** should control its own metric row. Compact/Full Research deliberately override and disable those toggles.
8. Test function-scope variable declarations and dynamic-request contexts, including the daily-mirror engine. A single invalid returned object can cascade into dozens of `iEngine` errors.
9. Follow [docs/VALIDATION.md](./docs/VALIDATION.md) in TradingView; distinguish native compile results from Python or static source inspections.
10. Do not claim improved predictive performance without a clearly reproducible out-of-sample evaluation.

## Limitations of published testing

This repository currently carries a native-editor checklist, not a validated TradingView end-to-end test suite. GitHub's ordinary file publishing process is not a substitute for Pine compilation, and the presence of a README or licence is not evidence that runtime behaviour has been independently verified.

The code derives from a public TradingView open-source script. Please respect all original authors' rights and the site's open-source reuse rules.
