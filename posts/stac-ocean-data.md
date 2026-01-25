# STAC for Oceanographic Data Discovery

*Finding ocean datasets without drowning in metadata.*

## The Problem

Oceanographic data is scattered: NOAA, Copernicus, NASA, institutional repositories. Each has different APIs, metadata formats, and access patterns. Finding the right dataset takes longer than analyzing it.

## STAC: SpatioTemporal Asset Catalog

[STAC](https://stacspec.org/) is a specification for describing geospatial data. Think of it as a standardized card catalog for Earth observation data.

Core concepts:
- **Item** — A single spatiotemporal asset (one satellite image, one day of buoy data)
- **Collection** — A group of related items (all Sentinel-2 images, all Argo floats)
- **Catalog** — Collections organized hierarchically
- **API** — Search across catalogs with standard queries

## Searching Ocean Data

```python
from pystac_client import Client

# Connect to a STAC API
client = Client.open("https://api.example.com/stac")

# Search by bounding box and time
results = client.search(
    collections=["sea-surface-temperature"],
    bbox=[-180, -60, 180, 60],
    datetime="2024-01-01/2024-12-31"
)

for item in results.items():
    print(item.id, item.properties["sst_mean"])
```

## Why STAC for Ocean Science

1. **Federated search** — Query multiple repositories with one API
2. **Cloud-native** — Assets can be COGs, Zarr, or cloud buckets
3. **Extensions** — Scientific metadata (units, quality flags, processing level)
4. **Reproducibility** — Exact asset versions, not just "SST data from NOAA"

## Getting Started

Major ocean data providers with STAC:
- [Microsoft Planetary Computer](https://planetarycomputer.microsoft.com/)
- [Element 84 Earth Search](https://www.element84.com/earth-search/)
- [C4IROcean Ocean Data Platform](https://oceandata.earth/)

---

*One API to find them all.*

[Source: C4IROcean-OceanDataPlatform](https://github.com/aaronjohnson/C4IROcean-OceanDataPlatform)
