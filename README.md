# CO2 time series — Le Mans

Weekly pipeline that pulls the last 90 days of real hourly atmospheric CO2 (ppm) for
Le Mans from Open-Meteo's Air Quality API, aggregates it to daily values, flags missing
days, smooths it (7-day rolling mean, exponential smoothing) and projects a linear
trend 7 days ahead. Results are published on GitHub Pages with an explicit note on
the projection's limits.

- `scripts/fetch_series.py`: API call, hourly to daily aggregation -> `data/series.csv`
- `scripts/analyze_series.py`: cleaning, smoothing, projection -> `docs/data/series_analysis.json`
- `.github/workflows/update_series.yml`: runs both every Monday, or on demand
