# Dask Patterns for Large Ocean Datasets

*Practical patterns for when pandas runs out of memory.*

## When Dask?

Your ocean dataset is:
- Larger than RAM
- Embarrassingly parallel (same operation on many files)
- Needs lazy evaluation (don't load until necessary)

## Pattern 1: Lazy Loading

```python
import dask.dataframe as dd

# Don't load yet — just build the task graph
df = dd.read_parquet("s3://bucket/ocean-data/*.parquet")

# Still lazy
filtered = df[df.depth > 100]

# NOW it loads (only the filtered rows)
result = filtered.compute()
```

## Pattern 2: Chunked NetCDF with Xarray

```python
import xarray as xr

# Chunk along time dimension
ds = xr.open_mfdataset(
    "data/*.nc",
    chunks={"time": 100, "lat": 500, "lon": 500}
)

# Operations are lazy
mean_sst = ds.sst.mean(dim="time")

# Compute when ready
mean_sst.compute()
```

## Pattern 3: Distributed Processing

```python
from dask.distributed import Client

# Local cluster (uses all cores)
client = Client()

# Or remote cluster
client = Client("scheduler-address:8786")

# Same code, now distributed
result = df.groupby("region").mean().compute()
```

## Gotchas

| Issue | Solution |
|-------|----------|
| Too many small files | Repartition: `df.repartition(npartitions=100)` |
| Memory spikes | Reduce chunk size |
| Slow shuffles | Persist before groupby: `df.persist()` |
| Silent failures | Check dashboard: `client.dashboard_link` |

## The Dashboard

Always run with the dashboard open:

```python
client = Client()
print(client.dashboard_link)  # http://localhost:8787
```

Watch for:
- Red tasks (errors)
- Memory pressure (yellow/red bars)
- Unbalanced workers

---

*Pandas syntax, terabyte scale.*

[Source: C4IROcean-OceanDataPlatform](https://github.com/aaronjohnson/C4IROcean-OceanDataPlatform)
