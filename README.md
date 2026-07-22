# TradingView Indicators

A collection of original Pine Script v5/v6 indicators, kept here as `.pine` files (paste directly into the TradingView Pine Editor). Each indicator is a single, self-contained script — no file depends on
another.

## Publishing status

TradingView moderation hides scripts that rehash a built-in tool, a
well-known public script, or someone else's proprietary method without
enough original content on top (see their [Script Publishing
Rules](https://www.tradingview.com/house-rules/?solution=43000590599)).
Files below marked 🚫 are kept here for reference/local use only — they
duplicate an existing built-in or public script closely enough that
republishing them isn't likely to pass review. Everything else (✅) is
original enough to publish, provided it ships with a detailed description
(see `PUBLISH_DESCRIPTIONS.md`).

## Trend & Moving Averages
| Status | File | What it does |
|---|---|---|
| 🚫 | [`Hull Suite.pine`](Hull%20Suite.pine) | Hull-family moving average (HMA/EHMA/THMA) — same name and formulas as the well-known public "Hull Suite" script. Not republished. |
| 🚫 | [`Exhaustion Count Heatmap.pine`](Exhaustion%20Count%20Heatmap.pine) | Consecutive-close buy/sell setup counter (1–13) with a bar-painting heatmap. The counting rule is Tom DeMark's proprietary TD Setup methodology (same reason `TD Sequential.pine` was removed). Not republished. |
| ✅ | [`ADX Flash + DMI Signals.pine`](ADX%20Flash%20%2B%20DMI%20Signals.pine) | Flags low-ADX ("flash") consolidation periods, tracks the breakout target price, and signals ADX/DMI breakouts, breakdowns, and trend exhaustion. |
| ✅ | [`Moving Averages + Cross Signals.txt`](Moving%20Averages%20%2B%20Cross%20Signals.txt) | Four configurable moving averages plus a slope-and-separation-filtered MA3/MA4 cross signal with optional preemptive (projected) firing. |

## Momentum & Relative Strength
| Status | File | What it does |
|---|---|---|
| ✅ | [`RSI with Divergences.pine`](RSI%20with%20Divergences.pine) | RSI with overbought/oversold zones, bull/bear divergence detection, bull/bear control zones, and zone-break alerts. |
| ✅ | [`RSI-MFI Momentum Oscillator.pine`](RSI-MFI%20Momentum%20Oscillator.pine) | MACD-style oscillator blending RSI momentum with MFI as a participation/confirmation weight. |
| ✅ | [`Cost Basis Deviation Ratio.pine`](Cost%20Basis%20Deviation%20Ratio.pine) | Short/mid/long-term price-to-moving-average ratios (an SMA-based cost-basis proxy, not on-chain data) with configurable over/undervalued zone shading. |

## Volatility
| Status | File | What it does |
|---|---|---|
| 🚫 | [`Bollinger Bands.pine`](Bollinger%20Bands.pine) | Standard Bollinger Bands with an optional %-beyond-band deviation label — functionally TradingView's built-in indicator. Not republished. |
| 🚫 | [`Historical Volatility Percentile.pine`](Historical%20Volatility%20Percentile.pine) | Price + volume historical-volatility percentile blend with regime-shift crossover signals. Derivative of kocurekc's open-source "HVVP" — the reused percentile-rank core isn't a clearly "small proportion" of the script per TV's open-source reuse rule. Not republished. |
| ✅ | [`Abs Distance MA.pine`](Abs%20Distance%20MA.pine) | Normalized % distance of price from its moving average, with SD bands, divergence detection, momentum fill, and z-score. |
| ✅ | [`ATR Signal.pine`](ATR%20Signal.pine) | Flags when price has stretched more than N ATR-multiples away from its moving average. |

## Volume & Order Flow
| Status | File | What it does |
|---|---|---|
| ✅ | [`Short Pressure Index.pine`](Short%20Pressure%20Index.pine) | Composite 0–100 short-pressure score from volume climax, MA deviation, and bearish-bar momentum, with optional crypto exchange routing and FINRA short-volume data for stocks. |
| ✅ | [`Move Strength Index.pine`](Move%20Strength%20Index.pine) | Relative volume as green/red column height (intensity scaled by percentile rank vs. the symbol's own history); flags high volume packed into an unusually narrow range as yellow "compression" — effort without result that often precedes a sharp move. |
| ✅ | [`Stacked Imbalances.pine`](Stacked%20Imbalances.pine) | Flags zones where N consecutive bars show high-volume, one-sided delta; tracks zone flips (trapped buyers/sellers). |
| 🚫 | [`Vector Candles.pine`](Vector%20Candles.pine) | PVSRA-style candle coloring for climax and above-average volume bars — mirrors the widely-circulated public PVSRA scripts. Not republished. |
| ✅ | [`Unfinished Auction Detector.pine`](Unfinished%20Auction%20Detector.pine) | Flags high-volume bars with a long rejection wick and tracks the level as a return target until price revisits it. |
| ✅ | [`Short Covering Gauge.pine`](Short%20Covering%20Gauge.pine) | Flags bars that broke to a fresh multi-bar low (or high) and then reversed sharply into the close, read as forced short covering (or forced late selling). |
| ✅ | [`Volume Nodes (HVN LVN) Tracker.pine`](Volume%20Nodes%20%28HVN%20LVN%29%20Tracker.pine) | Plots the top N highest- and lowest-volume price levels within a lookback window. |
| 🚫 | [`Visible Range Volume Profile.pine`](Visible%20Range%20Volume%20Profile.pine) | Full volume profile for the visible chart range — POC/VAH/VAL, up/down/delta coloring, multi-POC (volume-node) detection, and a stats table. Duplicates TradingView's own native Visible Range Volume Profile tool. Not republished. |
| 🚫 | [`Smart Money Flow.pine`](Smart%20Money%20Flow.pine) | Boundary-based %-of-range read relabeled as "Smart Money" / "Daytrader" / "Retail". Its own header discloses it as a simplified rewrite of "MCDX Plus" by Kent_RichProFit_93 — a cleanup, not new content. Not republished. |

## Market Structure & Levels
| Status | File | What it does |
|---|---|---|
| 🚫 | [`Pivot Points.pine`](Pivot%20Points.pine) | Classic pivot point / R1 / S1 levels for 4H, Daily, Weekly, Monthly, and Yearly timeframes — a smaller reimplementation of TradingView's built-in Pivot Points Standard. Not republished. |
| ✅ | [`Custom Anchored VWAP.pine`](Custom%20Anchored%20VWAP.pine) | Two independently configurable date-anchored VWAP lines. |
| ✅ | [`IPO VWAP.pine`](IPO%20VWAP.pine) | Cumulative VWAP calculated from the first available bar (i.e. since listing). |
| ✅ | [`Gap Classifier.pine`](Gap%20Classifier.pine) | Tracks unfilled gaps and classifies them as breakaway, runaway, exhaustion, or common based on volume and trend context. |

## Patterns
| Status | File | What it does |
|---|---|---|
| ✅ | [`Candlestick Patterns.pine`](Candlestick%20Patterns.pine) | Detects hammer, engulfing, morning/evening star, marubozu, piercing/dark cloud, and doji patterns, with optional volume confirmation. |

## Calendar & Utility
| Status | File | What it does |
|---|---|---|
| ✅ | [`Moon Cycles.pine`](Moon%20Cycles.pine) | Marks new/first-quarter/full/last-quarter lunar phases on the chart. |

## Usage
Each file is a complete Pine Script. Open the TradingView Pine Editor, paste the contents of a `.pine` file, and click "Add to Chart." No file requires any other file in this repo.

## Publishing on TradingView
See [`PUBLISH_DESCRIPTIONS.md`](PUBLISH_DESCRIPTIONS.md) for a ready-to-paste
description per ✅ script, written to satisfy TradingView's originality and
description house rules (what makes it original, what it does and how it's
calculated, and how to use it).
