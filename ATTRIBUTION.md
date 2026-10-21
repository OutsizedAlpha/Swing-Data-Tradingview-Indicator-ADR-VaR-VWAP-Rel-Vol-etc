# Attribution, source history and reuse

This repository is an **open-source derivative**. Attribution is important because the code and ideas evolved through several contributors and releases.

## Primary original author — jfsrev (Jeff)

- **Original work:** [Swing Data - ADR% / RVol / PVol / Float % / Avg $ Vol](https://www.tradingview.com/script/uloAa2EI-Swing-Data-ADR-RVol-PVol-Float-Avg-Vol/)
- **Publisher:** [jfsrev on TradingView](https://www.tradingview.com/u/jfsrev/)
- **Initial publication:** 24 April 2023; the public release history continues through December 2024.
- **Contribution:** original Swing Data table architecture, early ADR/ATR, volume/float, relative-volume and supporting dashboard concepts.

**Primary credit for the original indicator belongs to jfsrev.** OutsizedAlpha does not claim to have authored that initial source.

## Earlier projected-volume reference — Bob-Lablaw

The inherited source specifically credits **Bob-Lablaw** and links to [Projected Volume](https://www.tradingview.com/script/tFe9aLsd-Projected-Volume/). The current OutsizedAlpha version reworked the projection approach to use historical same-time volume completion, but the inherited acknowledgement is retained.

## Other upstream acknowledgements

The public release notes for jfsrev's original script thank **@DumbleDax** for work related to a market-cap discrepancy and **@lacedtrades** for raising it. Those acknowledgements are reproduced here as part of the public history, **not** as assertions that these individuals directly contributed source to the current OutsizedAlpha version.

TradingView's editor, Pine Script language, public documentation and market-data platform also underpin this implementation but are not authors of this derivative.

## Later extensions — OutsizedAlpha

Later OutsizedAlpha-directed development and review include:

- Additional daily and higher-timeframe same-time volume engines, incomplete-feed validation and adaptive sample disclosure.
- Regular-session anchored VWAP estimates and a maintained **percentage-only** distance from VWAP.
- Float and market-cap source reconciliation, gross turnover metrics, historical-float versus volume-proxy distinctions.
- Completed-return historical VaR, 5% Expected Shortfall, and sample-quality warnings.
- Alternative ADR range normalisation and price-displacement/turnover diagnostic metrics.
- Directional Efficiency, an estimated days-to-liquidate utility and a multi-position, multi-size, Custom/Compact/Full Research table with independent row toggles.
- Explicit handling of unavailable calibration data for the derived RS percentile.
- Public documentation, review notes and maintenance of this repository.

These features were implemented through iterations that involved substantial rewriting and corrections. **The Git history begins with the imported v6.3 snapshot; do not infer that all changes were independently developed in the initial commit or that upstream source authors endorse them.**

## Licence

The upstream TradingView publication is marked **open source**. TradingView's terms state that open-source script source defaults to the **Mozilla Public License 2.0** when no alternative licence is specified. We are distributing this derivative under MPL-2.0, with the [full text](./LICENSE) and a source-file notice.

- [TradingView open-source publication and reuse guidance](https://www.tradingview.com/pine-script-docs/writing/publishing/)
- [TradingView script publishing rules](https://www.tradingview.com/support/solutions/43000590599-script-publishing-rules/)
- [MPL-2.0 licence](https://www.mozilla.org/MPL/2.0/)

MPL-2.0 provides source-level obligations; it does not transfer ownership of upstream contributions or grant trademarks. **TradingView community publishing rules can impose additional conditions when republishing on TradingView itself.**

If you identify an omitted upstream author or a more specific applicable licence, open a GitHub issue with the source link and relevant passage. We will correct the credits or licence treatment appropriately.

*No association, sponsorship or endorsement by jfsrev, Bob-Lablaw, TradingView, IBD or Symbolik is claimed.*
