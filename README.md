# SDS-hackathon-2026

# First time handling big data sets
Before you do anything, read the following steps. it keeps your laptop from freezing and sets the "speed limit".

`import polars as pl`

## Prevents Polars from taking up every bit of CPU power
`os.environ["POLARS_MAX_THREADS"] = "4"`

## Don't "Read", just "Scan"
Never use pl.read_parquet(). You don't have enough RAM. Use scan_parquet() to create a LazyFrame. This acts as a "plan" rather than a "load."

This takes 0.001 seconds because it doesn't load the data yet

`df_lazy = pl.scan_parquet("data/aisdk-2024-1h.parquet")`

## Filter Early, Collect Late
The secret to working with "Big Data" on a small laptop is to throw away what you don't need before you bring it into memory.

Bad: Loading all 26 columns.

Good: Only selecting e.g. Latitude, Longitude, and MMSI.

## The "Go" Button
Your code will only actually run when you call .collect(). If it’s still slow, use streaming=True to process the data in small sips.

## Don't run your full analysis on 104 million rows every time. Test your logic on a sample first:
`df_sample = df_lazy.head(100_000).collect()`
