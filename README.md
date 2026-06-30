# Trade Journal Pro

A polished single-page trading journal for logging trades, reviewing calendar performance, tracking P/L, and exporting backups. It runs as plain static HTML and stores data in the browser with `localStorage`.

## Files

- `index.html` - main journal app
- `jornal.html` - compatibility redirect for the original filename

## Features

- Calendar view by month and year
- Trade form with symbol, side, setup, entry, exit, stop, target, risk, fees, grade, mood, and notes
- Net P/L, win rate, profit factor, average R, and streak metrics
- Monthly P/L and equity curve charts
- Trade log with filters
- CSV export and JSON backup/import
- Automatic migration from the original `Jan1`, `Feb2`, etc. localStorage data format
