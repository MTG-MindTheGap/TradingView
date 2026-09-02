# TradingView Publish Descriptions

Ready-to-paste descriptions for the TradingView "Description" field when
publishing each script. Each one covers what the script does and how it's
calculated (in plain language, not Pine), and how to use it.

---

## ADX Flash + DMI Signals

**Overview**
Reads two consecutive phases of the same ADX/DMI trend-strength cycle from
one shared calculation: a "flash" detector that catches the low-ADX setup
phase *before* a trend starts, and a DMI/ADX cross engine that catches
confirmation and exhaustion *after* one does. The flash detector's tracked
breakout target and the DMI signals both derive from the same `ta.dmi()`
call, and the flash target table tells you where the eventual DMI
breakout/breakdown is likely to originate from. Each half can be toggled off
independently if you only want one phase of the read.

**How it's calculated**
- *ADX Flash*: highlights every bar where ADX(14) sits below your threshold
  (default 13) — a proxy for a market that has stopped trending. When ADX
  crosses back above the threshold, the script records the price at the
  *last* bar still inside that low-ADX period as a breakout target, then
  tracks price against that level (with an optional table showing distance
  and age of each pending target) until it's touched.
- *DMI Signals*: a breakout/breakdown fires when ADX crosses above whichever
  DI line is currently subordinate while ADX itself is rising — i.e. the
  weaker side is being overrun by strengthening trend. Exhaustion signals
  fire when ADX, after climbing steadily above both DI lines for N bars,
  crosses the *dominant* DI line — read as trend strength peaking and
  starting to fade.

**How to use it**
Use the flash shading to spot compression zones worth watching for a
breakout; the tracked target price and table give you a level to set an
alert on. Use the breakout/breakdown arrows for trend-confirmation entries
and the exhaustion arrows as an early warning that a trending move may be
running out of steam. All four conditions have `alertcondition()` calls.

---

## Moving Averages + Cross Signals

**Overview**
A 4-line configurable moving-average display (SMA/EMA/RMA/VWMA/WMA, any
length/source) paired with a cross-signal engine for MA3/MA4 that filters
out weak or premature crosses and can fire ahead of the actual cross.

**How it's calculated**
A standard MA cross only tells you two lines touched — it says nothing
about whether they're separating cleanly or converging on a fakeout. This
script computes the per-bar slope of MA3 and MA4, projects both lines
forward by a configurable number of bars using that slope, and looks for
the cross on the *projected* series. A signal only fires when: the
projected lines are separating by more than an ATR-scaled minimum (kills
touch-and-reverse setups), both MAs' multi-bar slope and immediate
bar-over-bar direction agree with the signal, and price is on the correct
side of both MAs. At a 0-bar projection this collapses to a normal cross;
at 2+ bars it can flag the setup slightly before the lines actually cross.

**How to use it**
Set MA3/MA4 to your fast/slow pair (defaults: 9 and 21). Use "Preemptive
Bars Ahead" at 0 for confirmed crosses only, or 2–3 for earlier entries at
the cost of more false positives. "Min Cross Separation" controls how
strict the anti-fakeout filter is — raise it on choppy symbols. MA1/MA2 are
independent reference lines (defaults: 50/200) with no signal logic
attached. Buy/Sell labels come with matching alerts.

---

## RSI with Divergences

**Overview**
Standard RSI plus a divergence engine, "control zone" bands, and
zone-break alerts — three tools combined into a single read: momentum
level, momentum-vs-price disagreement, and a defined zone-loss trigger.

**How it's calculated**
- *Divergence*: the script tracks the running price and RSI extremes since
  the last confirmed RSI pivot. A bearish divergence flags when a new,
  higher price extreme occurs while RSI fails to make a correspondingly
  higher extreme (and vice versa for bullish) — i.e. price and momentum
  disagree at the extremes.
- *Control zones*: two bands (default 65–80 bullish, 20–35 bearish) mark
  where RSI has tended to hold during a healthy trend. Losing the bullish
  zone (RSI crossing back under its floor) or reclaiming out of the bearish
  zone are flagged as "watch" signals — full-opacity if a divergence fired
  in the prior 20 bars, dimmed otherwise, so confluence is visible at a
  glance.

**How to use it**
Use divergence labels as an early warning, not a standalone signal. Use the
control-zone bands to judge whether the current trend is intact (RSI
holding its zone) or weakening (RSI losing it) — the ▼/▲ zone-break arrows
are your actionable trigger, with brightness indicating whether a prior
divergence backs it up. All conditions have alerts.

---

## RSI-MFI Momentum Oscillator

**Overview**
A MACD-style oscillator, but instead of smoothing price directly, it
smooths a composite "genuine momentum" series built by weighting RSI's
directional bias with MFI's volume-based confirmation — the idea being that
momentum backed by participation is more reliable than momentum on RSI
alone.

**How it's calculated**
`rawGenuine = (RSI − 50) × (1 + ((MFI − 50) / 50) × confirmWeight)`. RSI's
distance from its midpoint sets the base direction/magnitude; MFI's
distance from its midpoint scales that value up when volume flow agrees
with the RSI bias and pulls it toward zero when it doesn't. That composite
series is then run through a standard fast/slow EMA and signal-line MACD
structure to produce an oscillator, signal line, and histogram.

**How to use it**
Read it like a MACD: histogram color/direction for momentum shifts, the
zero-line cross for a directional reset, oscillator/signal crosses for
entries. The "Show Raw Genuine Momentum" option plots the unsmoothed
composite if you want to see the RSI/MFI blend before EMA smoothing.
Increase "MFI Confirmation Weight" to make volume disagreement suppress the
signal more aggressively. Six alert conditions are included (zero crosses,
signal crosses, histogram turning).

---

## Cost Basis Deviation Ratio

**Overview**
Plots price relative to three independent moving-average lookbacks
(short/mid/long) as a ratio, so you can see at a glance whether price is
extended above or compressed below each cohort's average entry level, and
whether short-, mid-, and long-term readings agree or diverge.

**How it's calculated**
For each cohort length, the moving average of closing price over that
lookback is used as a simple proxy for that cohort's average cost basis;
the ratio `close / cohort MA` is then plotted directly, plus a smoothed
trend line of that ratio. A ratio of 1.0 means price sits exactly at that
cohort's average; above 1.0 means price is trading at a premium to it,
below 1.0 a discount. Configurable "Overvalued"/"Undervalued" thresholds
(defaults 1.5 / 0.5) shade the background whenever *any* cohort crosses
them.

**How to use it**
This is a lightweight, symbol-agnostic proxy for cost-basis positioning —
it is not on-chain realized value and doesn't require volume or holder
data, so it works on any market (equities, FX, crypto). Use agreement
across all three cohorts (all above 1.0, or all below) as a stronger signal
than any single line; use divergence between the short-term and long-term
ratio to spot short-term extensions within a longer-term trend. Tune the
Overvalued/Undervalued thresholds per symbol — a low-volatility index and a
small-cap will have very different normal ranges.

---

## Abs Distance MA

**Overview**
Normalizes price's distance from its moving average into a %, then treats
that % series as its own instrument — with its own SD bands, z-score,
momentum, divergence detection, and a timeframe-aware auto lookback — to
give a single read on "how stretched is price, statistically."

**How it's calculated**
`pct_dist = |close − MA| / MA × 100`. That series then gets its own rolling
mean and standard deviation (window length auto-scales with chart
timeframe — 1000 bars on intraday down to 200 on weekly+, so the sample
size stays meaningful across timeframes), from which 1/2/3-SD bands, a
z-score, and a dynamic threshold (mean + 1 SD, or a fixed level) are
derived. Momentum is `pct_dist` minus its value N bars ago; divergence
compares pivot highs/lows of price against pivot highs/lows of `pct_dist`
the same way RSI divergence works, just applied to the distance series
instead.

**How to use it**
Rising `pct_dist` toward or through the 2-3 SD bands flags a statistically
stretched move; falling back toward zero (or the −1 SD "compression" band)
flags mean-reversion. Divergence markers catch cases where price keeps
extending but the *rate* of extension is fading. Toggle "Auto SD Lookback"
off if you want a fixed, manually-set statistical window instead of the
timeframe-scaled default.

---

## ATR Signal

**Overview**
A single, focused extension signal: flags when price has stretched further
from its moving average than N ATR%-multiples, using ATR as a
volatility-normalized yardstick rather than a fixed percentage.

**How it's calculated**
`atrp = ATR(14) / close × 100` gives ATR as a % of price. The distance
between the configurable source and a configurable MA is then expressed as
a multiple of that ATR%: `(source − MA) / MA / atrp × 100`. When that
multiple exceeds your threshold (default 12), a dot plots. Because the
distance is scaled by the instrument's own ATR%, the same threshold applies
sensibly to both a low-volatility index and a volatile small-cap without
retuning.

**How to use it**
Use it as a simple, single-purpose overextension flag — pair it with your
own trend/entry logic rather than trading it standalone. Raise the
threshold for fewer, more extreme signals; lower it for more sensitivity.
MA type/length/source are fully configurable, so you can test the signal
against a fast or slow baseline. Includes an alert condition.

---

## Short Pressure Index

**Overview**
A composite 0–100 "short pressure" score built from three independently
computed components (volume climax, MA deviation, and bearish-bar
momentum), with optional automatic exchange routing for crypto and FINRA
short-volume data substitution for stocks — so the same score means
something comparable across very different asset classes.

**How it's calculated**
1. *Volume pressure*: the fraction of high-significance volume (climax or
   above-average bars, per a PVSRA-style multiplier check) that occurred on
   bearish closes over a rolling window. For US stocks with FINRA short
   volume enabled, this component is swapped for the actual reported
   short-volume ratio instead.
2. *MA deviation pressure*: how far below (in ATR multiples) price sits
   from its moving average, rescaled to 0–100 over a rolling window.
3. *Momentum pressure*: the % of the last N bars that closed bearish.

The three are averaged and smoothed with an EMA to produce the SPI line,
which gets its own signal-line EMA and histogram. For crypto, price/volume
can be routed to a more liquid exchange feed (e.g. Bitfinex) since the
chart's native feed may be sparse; unsupported symbol types can be
suppressed entirely rather than plotting a meaningless value.

**How to use it**
Readings above the high threshold (default 70) suggest broad short-side
pressure building across all three components; below the low threshold
(default 30) suggests minimal short pressure. The histogram and signal-line
crosses give earlier, more granular entries than waiting for a threshold
cross. For US equities, "Use FINRA Short Volume" swaps in real reported
short-sale data for a more direct read than the volume-climax proxy alone.

---

## Move Strength Index

**Overview**
A relative-volume indicator: column height shows the relative-volume
multiple, color intensity separately highlights which of those bars are
actually unusual for that specific symbol, and a distinct compression color
flags the specific case where heavy volume produces almost no range — a
classic effort-without-result signature that often precedes a sharp move.

**How it's calculated**
Relative volume is current volume divided by a smoothed (RMA) average of
the *prior* bars — the still-forming bar is excluded from its own
baseline (otherwise its own volume dampens the reading), and the smoothed
baseline decays gradually rather than using a fixed rolling window,
avoiding the "step" artifact that occurs when an old outlier bar abruptly
exits a fixed-length SMA window. Color intensity is driven by the bar's
percentile rank against its own trailing history (default 100 bars), not a
fixed multiple — percentile rank self-calibrates so the loudest days for
any given symbol are always vivid and quiet days are always dim.

Direction is close vs. open — the same test used to color the candles on
the price chart itself, so MSI's green/red always agrees with what the
candle next to it shows.

A bar is recolored **yellow (compression)** when its volume ranks high
(top percentile, same threshold used for alerts) but its high-low range
is unusually narrow relative to ATR — heavy participation that isn't
moving price, which typically means one side is being absorbed at that
level. The live, still-forming bar is dimmed by default, since its
partial volume will always understate true relative volume until the
bar closes.

**How to use it**
Column height is the relative-volume multiple directly — a bar at 2.0
traded at twice its recent baseline volume. Brightness tells you whether
that reading is actually unusual for this symbol; a tall but dull column
means the symbol just runs volatile volume normally, while a vivid
column is a genuine standout. Green/red tells you which side controlled
the bar. A yellow bar is the one to watch closely — high volume with
nothing to show for it in price, often the setup before a breakout or
breakdown. Alerts fire separately for strong buying, strong selling, and
compression. On a symbol with no real volume data, the script raises a
clear error instead of rendering a blank pane.

---

## Stacked Imbalances

**Overview**
Flags zones where several consecutive bars all show high-volume, one-sided
order flow in the same direction — a footprint-chart concept ("stacked
imbalances") adapted to work from OHLCV bars alone — and tracks when a
zone gets invalidated by an opposing stack forming at the same price
(a "flip," read as trapped buyers/sellers).

**How it's calculated**
Each bar's volume is split into an estimated buy/sell component using where
the close sits within the bar's high-low range (`bullFraction = (close −
low) / range`), giving a per-bar delta. A bar counts as an "imbalance bar"
if its volume exceeds a multiple of its rolling average AND its delta is
one-sided (optionally also requiring the bar to close in that direction).
When `stackMinCount` (default 3) consecutive imbalance bars in the same
direction occur, a zone is drawn spanning their combined high/low. If a new
opposing-direction stack later overlaps an existing zone's price range, the
older zone is deleted and the event is marked as a "flip" (rendered in a
distinct color) — the read being that the side which built the original
zone got trapped and reversed.

**How to use it**
Fresh zones mark areas of aggressive one-sided participation — useful as
support/resistance or continuation zones. Flip zones (color-coded
separately) are the higher-information event: they suggest the opposing
side absorbed the imbalance and reversed it. "Require Bar Close in Stack
Direction" tightens the imbalance-bar definition; "Delete Zone on
Close-Through" auto-invalidates a zone once price closes through it rather
than just through a flip. Alerts are provided for fresh zones and for
flips specifically.

---

## Unfinished Auction Detector

**Overview**
Flags a specific market-structure pattern: a high-volume bar that gets
strongly rejected at one extreme, leaving a long wick — read as an
"unfinished auction" that the market is likely to revisit — and tracks that
price level as a live target until price returns to it or it ages out.

**How it's calculated**
A bar qualifies when its volume exceeds a multiple of its rolling average
AND the wick on one side is at least a configurable fraction of the bar's
total range (default 40%) with the close on the opposite side. The
rejected extreme (the low for a bullish rejection, the high for a bearish
one) is drawn as a level and tracked; "returned" is defined as price coming
within an ATR-scaled tolerance of that level, at which point the level is
automatically removed. Up to a configurable number of the most recent
levels are tracked at once, oldest dropped first.

**How to use it**
Treat active (undeleted) levels as return targets — price left that level
unresolved with strong participation behind it. "Touch Sensitivity" scales
how close price needs to come before a level counts as revisited (in ATR
multiples, so it adapts to the instrument's volatility automatically).
Tune "Min Volume to Flag" and "Min Wick Ratio" to control how strict the
rejection criteria are.

---

## Short Covering Gauge

**Overview**
Marks bars where price broke to a fresh multi-bar low early on and then
reversed sharply into the close — read as forced short covering rather
than fresh buying conviction — with a symmetric bearish case for early
strength that gets sold back down before the close. Built on intrabar
(lower-timeframe) data so the same logic applies on any timeframe: daily
sub-bars inside a weekly bar, hourly sub-bars inside a daily bar, and so
on.

**How it's calculated**
For each bar, the script pulls lower-timeframe sub-bars and splits them
into an early segment and a late segment (default: the last 40% of
sub-bars, e.g. the final 2 of 5 trading days of a week). It measures how
far the early segment traded below the bar's open (the drawdown) or above
it (the rally). That early move only qualifies if it clears an ATR floor
and breaks beyond the high/low of the prior N bars (default 20) — so a
routine mid-range wiggle doesn't count, only a genuine extreme relative to
recent structure. A bullish signal fires when price was still below a
reclaim level (a configurable fraction of the drawdown, default 60%,
measured from the early low back toward the open) going into the late
segment, and the close then reclaims that level — i.e. the recovery
specifically happened late, not gradually all bar. The bearish case is the
mirror image on an early rally that gets sold back down. A signed
background reading plus a "% reclaimed" / "% given back" label shows the
size of the move on qualifying bars.

**How to use it**
Best read on higher timeframes (weekly/daily) where the close reflects
deadline-driven flow — margin calls, MOC imbalances, vol-control/CTA
rebalancing — rather than opinion. A green triangle below a bar flags a
fresh low that got aggressively bought back before the close; a red
triangle above a bar flags a fresh high that got sold back down. "Fresh
Extreme Lookback" controls how significant the early extreme must be
before it counts; "Reclaim Level" controls how full a reversal is
required. Signals only stamp on confirmed bars by default, since an
intrabar reading can still flip before the close. Treat it as a pressure
gauge, not a standalone entry trigger — combine with your own
trend/structure read.

---

## Volume Nodes (HVN/LVN) Tracker

**Overview**
A lightweight proxy for high/low volume nodes: rather than building a full
volume profile, it directly ranks the highest- and lowest-volume individual
bars within a lookback window and plots their price levels — a much
cheaper calculation that still surfaces the same broad areas of
participation/avoidance a profile would show, at the cost of precision.

**How it's calculated**
Over the lookback window, every bar's volume and midpoint price
(`(high+low)/2`) are collected into arrays, sorted by volume, and the top N
(for HVN) or bottom N (for LVN) bars' price levels are drawn as extending
lines with rank-numbered labels. HVN and LVN lookback, count, and line
style are all independently configurable.

**How to use it**
HVN levels mark bars where the most volume traded and tend to act as
magnets/support-resistance; LVN levels mark bars with the least volume and
tend to be areas price moves through quickly. This is explicitly a
per-bar proxy, not a true price-bucketed volume profile — useful for a
fast, low-overhead read of where participation clustered without the cost
of a full profile calculation.

---

## Custom Anchored VWAP

**Overview**
Two independently configurable, date-anchored VWAP lines on one chart —
built specifically so you can compare two different anchor points (e.g.
this year's open vs. a specific earnings date, or two prior swing lows)
without switching tools or re-anchoring a single line back and forth.

**How it's calculated**
Each VWAP anchors to a user-set year/month/day, accumulating
`Σ(typical price × volume) / Σ(volume)` from that date forward. The
accumulation runs on the daily timeframe internally (via
`request.security`) regardless of your chart's own timeframe, so the
anchor date lines up correctly whether you're viewing 5-minute bars or
weekly bars.

**How to use it**
Set each VWAP's anchor date independently in its input group. Useful for
comparing how price has behaved relative to two different reference points
simultaneously — e.g. year-to-date VWAP vs. VWAP since a specific catalyst
date — without needing two separate indicator instances.

---

## IPO VWAP

**Overview**
A cumulative VWAP anchored to the very first available bar of a symbol's
history — i.e. its listing/IPO date — with no manual anchor date required,
purpose-built for judging whether a newly listed or recently IPO'd stock is
trading above or below the volume-weighted average price paid by everyone
who's held it since it started trading.

**How it's calculated**
`Σ(typical price × volume) / Σ(volume)`, accumulated via `ta.cum()` from
the first bar the symbol has data for, computed on the daily timeframe and
mapped back to the chart's own timeframe. A volume-data check suppresses
the plot for symbols without usable volume rather than showing a
meaningless flat line.

**How to use it**
No configuration needed beyond color/width — the anchor is automatic. Use
it as a since-inception cost-basis reference: price sustained above IPO
VWAP suggests the average holder is in profit; sustained below suggests the
opposite. Most useful on stocks/tokens with a limited trading history where
a manually-anchored VWAP would need frequent re-anchoring anyway.

---

## Gap Classifier

**Overview**
Tracks unfilled price gaps and classifies each one — breakaway, runaway,
exhaustion, or common — based on the volume and trend context the gap
occurred in, rather than just marking "there's a gap here."

**How it's calculated**
A gap is detected when a new session's low opens above (or high opens
below) the prior period's close on your chosen gap timeframe. Each gap is
scored on: its volume ratio vs. a rolling average, whether it broke outside
the recent price range, whether ADX was above a trending threshold at the
time, and whether the existing trend (ADX above threshold, tracked as a
running bar-count) was already "mature" (long-running) when the gap
occurred. Those four inputs feed a classification: high volume + breaks the
range + trend not yet mature → breakaway; high volume + established trend
+ not yet mature → runaway; high volume + a mature/extended trend →
exhaustion; anything else → common. Each gap box tracks its own fill % in
real time and is removed once it fills past 50% or exceeds a max-age limit.

**How to use it**
Breakaway gaps (fresh volume, breaking structure) suggest a new trend
starting; runaway gaps (volume + existing trend, not yet extended) suggest
continuation; exhaustion gaps (volume + an already-extended trend) suggest
the move may be near its end; common gaps carry the least trend
information and are more likely to simply fill. Colors are independently
configurable per class. Turn off "Enable Gap Classification" to fall back
to plain unfilled-gap tracking without the volume/trend read.

---

## Candlestick Patterns

**Overview**
Detects twelve classic single/multi-bar candlestick patterns (hammer,
inverted hammer, bullish/bearish engulfing, morning/evening star,
marubozu, piercing line, dark cloud cover, shooting star, hanging man,
doji) from raw OHLC geometry, with an optional volume-confirmation filter
and overlap suppression between patterns that would otherwise both fire on
the same bar.

**How it's calculated**
Each pattern is defined by explicit body/wick/range ratio thresholds
relative to the bar's own range and the recent average body size (e.g. a
hammer requires a lower wick more than 2× the body, an upper wick under
30% of the body, and a body at least 30% of the recent average) — not a
single generic "small body, long wick" catch-all. Overlapping patterns are
explicitly de-conflicted (e.g. a bullish engulfing bar won't also fire as a
marubozu on the same bar). The optional volume filter requires the bar's
volume to exceed a configurable multiple of its average AND exceed the
prior two bars, before any pattern is allowed to label.

**How to use it**
Enable "Require Volume Confirmation" to filter out patterns on
low-participation bars — a common source of false pattern signals. Label
style is configurable between label markers, plain text, and arrows, with
adjustable size and vertical offset (in ATR multiples, so labels don't
crowd price on volatile symbols).

---

## Moon Cycles

**Overview**
Marks the four primary lunar phases (new, first quarter, full, last
quarter) directly on the chart using an astronomical calculation, for
traders who track lunar-cycle-based market sentiment/timing frameworks.

**How it's calculated**
Uses the standard synodic month constant (29.530588853 days) against a
known reference new moon (6 Jan 2000) to compute a continuous lunar-cycle
phase fraction (0 to 1) for the current bar's timestamp, then flags a phase
transition whenever that fraction crosses a phase boundary (0.0 = new,
0.25 = first quarter, 0.5 = full, 0.75 = last quarter) between the current
and prior bar.

**How to use it**
This isn't a trading signal on its own — it's a calendar overlay for traders
who want lunar phases visible alongside price action as one input among
others. Label text is configurable (emoji only, with meaning, with date, or
all three) and vertical lines can optionally mark new/full moons for easier
visual alignment with price swings.
