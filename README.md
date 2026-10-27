# ATLAS Trading Bot

ATLAS is a learning project: a Python trading bot that will eventually analyze three
micro futures markets:

| Market      | Symbol |
|-------------|--------|
| Gold        | MGC    |
| Bitcoin     | MBT    |
| Nasdaq-100  | MNQ    |

## Status: simulation only

- Uses **hard-coded, artificial prices**. No market data feed.
- **No real money, no live orders, no broker connection, no paid APIs.**
- The signals are educational examples. They are **not** trading advice, and the
  strategy has **not** been shown to be profitable.

## What it does today

`bot.py` holds a few sample prices per market. For each market, `analyze_market`:

1. Averages every price except the latest one.
2. Compares the latest price to that average:
   - above → `WATCH LONG`
   - below → `WATCH SHORT`
   - equal → `WAIT`

"WATCH" means "worth a look", not "place a trade".

Known limitation: a list with fewer than two prices raises `ZeroDivisionError`.

## Requirements

- Python 3.11+ with a virtual environment in `.venv`
- `bot.py` uses only the standard library
- `data_reader.py` needs `pandas`; tests need `pytest`

## Run the bot

```
python bot.py
```

Expected output:

```
ATLAS Trading Bot Started
MODE: SIMULATION ONLY
--------------------------
Gold: WATCH LONG
Bitcoin: WATCH SHORT
Nasdaq: WAIT
```

## Read and validate market data

```
.venv\Scripts\python.exe data_reader.py
```

Loads `data/gold_sample.csv`, runs `validate_candles()` and prints the candles,
`Validation: PASSED/FAILED` with any errors, any gaps, and the candle count.
Exits with code 1 if validation fails.

Validation rejects: missing columns; missing, invalid or timezone-less
timestamps; duplicate or out-of-order timestamps; missing, non-numeric or
non-positive prices; high/low inconsistent with open/close; missing or negative
volume. Timestamps are converted to UTC.

Gaps between candles are reported **separately, not as errors**: real futures
data has legitimate session breaks, which will need an exchange calendar.

## Run the tests

From the project folder:

```
.venv\Scripts\python.exe -m pytest -v
```

## Project layout

```
Atlas/
├── bot.py                    # sample data, analysis function, entry point
├── data_reader.py            # load_candles, validate_candles, report
├── data/
│   └── gold_sample.csv       # artificial 5-minute gold candles
├── tests/
│   ├── __init__.py
│   ├── test_bot.py           # tests for analyze_market and bot output
│   └── test_data_reader.py   # tests for loading and validation
├── README.md
└── .gitignore
```
"# Atlas__Project" 
