# H3 + Ocean Data: Hexagonal Geospatial for Marine Science

*Why hexagons are better than squares for ocean analysis.*

## The Problem

Ocean data comes from everywhere: satellites, buoys, ships, drones. Each source has different resolutions and coordinate systems. Joining them is painful.

## Enter H3

[H3](https://h3geo.org/) is Uber's hexagonal hierarchical spatial index. Instead of lat/long points or square grids, it divides Earth into hexagons at multiple resolutions.

Why hexagons?
- **Uniform distance** — neighbors are equidistant (squares have diagonal problems)
- **No edge effects** — hexagons tile without gaps at any scale
- **Hierarchical** — zoom in/out with consistent parent-child relationships

## Ocean Use Cases

### Multi-Dataset Joins

```python
import h3

# Convert any lat/long to a hex cell
cell = h3.latlng_to_cell(lat, lng, resolution=5)

# Now join datasets by cell ID, not fuzzy coordinates
chlorophyll_df['h3'] = chlorophyll_df.apply(
    lambda r: h3.latlng_to_cell(r.lat, r.lng, 5), axis=1
)
zooplankton_df['h3'] = zooplankton_df.apply(
    lambda r: h3.latlng_to_cell(r.lat, r.lng, 5), axis=1
)

merged = chlorophyll_df.merge(zooplankton_df, on='h3')
```

### Resolution Selection

| Resolution | Hex Edge | Use Case |
|------------|----------|----------|
| 3 | ~70 km | Basin-scale patterns |
| 5 | ~8 km | Regional analysis |
| 7 | ~1 km | Coastal studies |
| 9 | ~150 m | High-res satellite |

### Aggregation

```python
# Aggregate to coarser resolution
fine_cells = df['h3_res7'].unique()
coarse_cells = [h3.cell_to_parent(c, 5) for c in fine_cells]
```

## Why Not Squares?

At ocean scales, square grids distort near poles. H3's icosahedral projection maintains consistent cell areas globally. For marine science spanning latitudes, this matters.

---

*Hexagons: the bestagon for ocean data.*

[Source: C4IROcean-OceanDataPlatform](https://github.com/aaronjohnson/C4IROcean-OceanDataPlatform)
