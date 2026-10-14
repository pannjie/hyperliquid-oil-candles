# Hyperliquid oil perpetuals: 1-minute candles, 27 Feb – 2 Mar 2026

One-minute OHLCV candles for the three oil perpetual markets that traded on Hyperliquid
(builder-deployed / HIP-3 markets) over the weekend of 28 Feb – 1 Mar 2026, when real
oil futures were closed.

| File | Market | Notes |
|---|---|---|
| `xyz_CL_1m_complete.csv` | `xyz:CL` (trade[XYZ], WTI) | weekend price cap at 70.654 (+5% on a 67.2895 reference) |
| `flx_OIL_1m_complete.csv` | `flx:OIL` | weekend cap at 71.77 (+1/15 on the same reference) |
| `km_USOIL_1m_complete.csv` | `km:USOIL` | no cap; traded ~17–31% above the other two |

**Period:** 2026-02-27 20:00 to 2026-03-02 01:59 UTC, 3,240 minutes per file, no gaps.

## Columns

| Column | Meaning |
|---|---|
| `time` | start of the minute (UTC) |
| `o`, `h`, `l`, `c` | open, high, low, close price (USD per barrel) |
| `v` | volume, in barrels (contract units), each trade counted once |
| `n` | number of trades |

Minutes with no trades are included as flat candles at the previous close, with `v = 0`
and `n = 0`, the same convention as Hyperliquid's own candle API.

## Source and method

Built from every individual fill in Hyperliquid's node archive
(`s3://hl-mainnet-node-data/node_fills_by_block/hourly/`, requester-pays). Each trade
appears twice in the archive (buyer and seller); trades were de-duplicated on trade ID
before building candles.

Checked against Hyperliquid's 2-hour candles from its API: for `xyz:CL`, 25 of 27
two-hour candles match exactly on all fields; the other two contain liquidation bursts
that the API's candles undercount. `km:USOIL` matched 26/27 and `flx:OIL` 23/27, with
differences of the same kind.
