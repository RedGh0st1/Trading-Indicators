# Trading Indicators

A collection of **TradingView Pine Script v6 indicators** for studying liquidity sweeps, market structure, candle ranges, fair value gaps, and multi-timeframe price action.

The scripts cover ICT-style workflows including the 2022 Model, Candle Range Theory (CRT), Turtle Soup, Power of Three (Po3), and inversion fair value gaps. Each script is a standalone chart overlay—load the tools you need rather than combining their source into one indicator.

> **Indicators, not automated strategies.** These scripts draw chart context, setups, and—in some cases—trade levels or alerts. They do not provide Strategy Tester backtests or execute broker orders.

## Contents

- [Indicator library](#indicator-library)
- [Getting started](#getting-started)
- [Configuration](#configuration)
- [Alerts](#alerts)
- [Limitations and validation](#limitations-and-validation)
- [Terminology](#terminology)
- [Contributing](#contributing)
- [Attribution and licensing](#attribution-and-licensing)
- [Disclaimer](#disclaimer)

## Indicator library

All nine scripts declare Pine Script v6 and use `overlay = true`.

| Indicator | Source | Main features |
| --- | --- | --- |
| **2022 Model Pro+ \| ICTKESIK** | [2022 model pro+](2022%20model%20pro%2B) | Liquidity sweeps, SMT divergence, market structure shifts, displacement FVGs, breaker blocks, HTF FVGs, session filters, setup grading, entry/stop/target levels, and a checklist. |
| **CISD PRO+** | [CISDPROPLUS.pine](CISDPROPLUS.pine) | Change in State of Delivery lines and projections, session high/low levels, HTF FVG context, confluence filters, and an information table. |
| **CRT Pro+** | [CRTPROPLUS.pine](CRTPROPLUS.pine) | Two model modes, automatic or custom timeframe pairing, HTF candle panels, CRT boxes, sweep lines, CISD, order-block lines, SMT detection, and a dashboard. |
| **Fractal with PS** | [FractalwithPS.pine](FractalwithPS.pine) | Automatic, quarterly, preset, or custom fractal alignment; HTF candle projections; liquidity sweeps; live setup tracking; CISD; and optional price projections. |
| **Bandz Po3 Profiler** | [Po3-profile.pine](Po3-profile.pine) | Three HTF candle lanes, C1–C4 profiling, T-Spot, PSP, SMT/SSMT, CISD, FVG/volume-imbalance displays, liquidity levels, and timeframe ladders. |
| **Range - Raid - Run [BYSSB]** | [RRR.pine](RRR.pine) | Candle range highs/lows and midpoint, range-sweep context, FVGs touched by CRT sweeps, CISD buy/sell signals, and a timeframe dashboard. |
| **Turtle Soup model** | [Ts-ModelICT.pine](Ts-ModelICT.pine) | Sweep and double-purge context, market structure confirmation, inner-sweep retests, FVG/iFVG confluence, liquidity levels, HTF candle panels, projections, and session filters. |
| **iFVG Ultimate+ Deluxe \| DodgysDD** | [ifvg Ult deluxe](ifvg%20Ult%20deluxe) | Single/series inversion detection, setup grades, liquidity sweeps, HTF FVG alignment, SMT/SSMT, entry/stop/target displays, suggested contract sizing, volume profile, and new-week opening gaps. |
| **Sweep Reversal Map [Herman]** | [sweep_reversal.pine](sweep_reversal.pine) | Sweep/reclaim/structure-break reversal zones on the chart timeframe and two configurable HTF layers, developing/confirmed displays, ATR-based thresholds, and confirmation alerts. |

**Where to start:**

- **Liquidity reversals:** Sweep Reversal Map or the Turtle Soup model.
- **Candle ranges and raids:** CRT Pro+ or Range - Raid - Run.
- **Delivery shifts:** CISD PRO+.
- **Multi-timeframe context:** Fractal with PS or Bandz Po3 Profiler.
- **Broader setup checklists:** 2022 Model Pro+ or iFVG Ultimate+ Deluxe.

## Getting started

### Load a script in TradingView

1. Open a source file from the library above, then select **Raw** to view the plain-text code.
2. Copy the entire script, including its `//@version=6` directive.
3. Open a chart on [TradingView](https://www.tradingview.com/) and open the **Pine Editor**.
4. Create a new indicator script and replace the editor's default code with the copied source.
5. Save the script, then select **Add to chart**.
6. Open the indicator's **Settings** to review timeframe alignment, session filters, symbols, and display options.

The files `2022 model pro+` and `ifvg Ult deluxe` have no `.pine` extension, but contain Pine Script source and can be copied into the editor in the same way.

**Known source issue:** `Ts-ModelICT.pine` currently starts with `/ This Pine Script...` rather than a valid `//` comment. Change that initial `/` to `//` in your editor before compiling. This README does not modify the indicator source.

### Clone the collection locally

```bash
git clone https://github.com/RedGh0st1/Trading-Indicators.git
cd Trading-Indicators
```

No local package installation is needed to copy and use the scripts in TradingView. Pine Script runs on TradingView, not as a standalone Python or JavaScript program.

## Configuration

Settings differ between indicators. Before interpreting a setup, review:

| Setting | What to check |
| --- | --- |
| **Chart and higher timeframes** | Confirm that the selected pairing suits the chart. Where a setting requires a higher timeframe, choose one above the chart timeframe. Automatic mappings are script-specific. |
| **Sessions and timezone** | Check the timezone and each enabled session or killzone. Several scripts use New York time, but their session windows are not identical. |
| **Correlated symbols** | Verify automatic SMT symbol selection or choose a manual comparison symbol. Data access and symbol availability can affect results. |
| **Bias and confluence** | Direction, grade, FVG, sweep, and session filters can suppress setups. A quiet chart does not necessarily mean the indicator failed. |
| **History and drawing limits** | Adjust retained setups, boxes, lines, and HTF candles to balance context with chart readability and platform limits. |
| **Risk and projections** | Treat displayed entries, stops, targets, and contract quantities as calculated references—not broker fills or verified account risk. |

Start with one indicator, confirm its behavior in replay and live observation, then add complementary tools if needed.

## Alerts

Alert support is implemented differently across the collection:

| Script | Alert interface in source |
| --- | --- |
| 2022 Model Pro+ | Named `alertcondition()` events |
| CISD PRO+ | Dynamic `alert()` calls |
| CRT Pro+ | Named `alertcondition()` events |
| Fractal with PS | Dynamic `alert()` calls |
| Bandz Po3 Profiler | No `alert()` or `alertcondition()` calls present |
| Range - Raid - Run | Both named conditions and dynamic calls |
| Turtle Soup model | Both named conditions and dynamic calls |
| iFVG Ultimate+ Deluxe | Dynamic `alert()` calls |
| Sweep Reversal Map | Named `alertcondition()` events |

To create an alert:

1. Add the script to your chart and enable any relevant in-script alert toggles or time windows.
2. Open TradingView's **Create alert** dialog and select the indicator under **Condition**.
3. Select a named event for `alertcondition()` alerts, or **Any alert() function call** for dynamic alerts.
4. For named conditions, use **Once Per Bar Close** when you want bar-close evaluation. Dynamic calls use the frequency specified in the script.
5. Configure notifications and create the alert.

TradingView alerts retain a snapshot of the script and its inputs. After changing the source, indicator settings, symbol, or timeframe, recreate the alert to use the updated configuration.

## Limitations and validation

- **Source inspection is not compilation.** This documentation describes the checked-in code; it does not certify that every script compiles or behaves correctly in TradingView.
- **Developing setups can change.** Live sweeps, open candles, and unconfirmed HTF data may update or disappear before confirmation. Do not assume the whole collection is non-repainting.
- **Pivot confirmation takes time.** Pivot-based structures require subsequent bars; a marker anchored to an earlier candle was not necessarily available on that candle.
- **Visible history can be selective.** History limits and status filters can hide prior setups. In particular, iFVG Ultimate+ Deluxe defaults to `Hide Losing Setups = true`; turn that off when reviewing both winners and losers. A clean historical chart is not performance evidence.
- **Data and platform limits matter.** Correlated-symbol requests, lower-timeframe data, chart history, drawing limits, and TradingView plan restrictions may affect what is available or displayed.
- **Projections are not fills.** Displayed targets, setup status, and suggested sizing do not account for all real-world execution costs, slippage, or broker constraints.

Validate behavior on your intended instrument and timeframe before using any output in a trading decision.

## Terminology

| Term | Meaning |
| --- | --- |
| **ICT** | Inner Circle Trader; the trading framework referenced by several scripts. |
| **CRT** | Candle Range Theory. |
| **CISD** | Change in State of Delivery. |
| **MSS** | Market Structure Shift. |
| **FVG / iFVG** | Fair Value Gap / inversion Fair Value Gap. |
| **HTF / LTF** | Higher timeframe / lower timeframe. |
| **SMT / SSMT** | Intermarket divergence / sequential SMT divergence, as implemented by each script. |
| **Po3** | Power of Three: accumulation, manipulation, and distribution. |
| **BSL / SSL** | Buyside liquidity / sellside liquidity. |
| **CE** | Consequent Encroachment; the midpoint of a price array such as an FVG. |
| **PDH / PDL** | Previous day high / low. |
| **NWOG** | New Week Opening Gap. |

## Contributing

Bug reports and focused improvements are welcome through [issues](https://github.com/RedGh0st1/Trading-Indicators/issues) and pull requests.

For a reproducible report, include:

- The exact source filename and revision, if known.
- TradingView symbol, chart timeframe, and relevant indicator settings.
- Expected behavior versus observed behavior.
- The full Pine Editor error message, or a screenshot with the relevant bars and timestamps.

Keep changes scoped to the affected script, preserve attribution and license notices, and test changes in TradingView. Describe any changes to confirmation timing, historical drawings, or alerts.

## Attribution and licensing

Script titles and notices include names such as ICTKESIK, DodgysDD, BYSSB, Herman, Bandz, and RedGhost1224. Those names are retained here as they appear in the source; inclusion in this collection does not imply endorsement or common authorship.

There is currently **no repository-wide `LICENSE` file**. `FractalwithPS.pine`, `sweep_reversal.pine`, and the header of `Ts-ModelICT.pine` reference the [Mozilla Public License 2.0](https://mozilla.org/MPL/2.0/). Do not assume that those notices license every other file. Check each script's notices and obtain permission where necessary before redistributing, relicensing, or using code commercially.

## Disclaimer

These indicators are provided for educational and research purposes, not as financial advice or a promise of returns. Trading involves risk, including the loss of capital. Historical chart patterns, alerts, and calculated levels do not guarantee future results. You are responsible for validating the tools and managing your own trading risk.
