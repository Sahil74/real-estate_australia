# Property Orientation Analysis

This project calculates the facing direction of residential properties in Sydney suburbs and combines full addresses from separate components. It produces a CSV where each row represents one property, showing its address and orientation.

## Features

* Calculates property orientation from cadastral polygons.
* Handles MultiPolygons by using the largest polygon.
* Combines GNAF address fields into a single readable `full_address`.
* Spatially joins property points to their polygons.
* Outputs a CSV with `full_address` and `orientation` for easy use by investors or analysts.

## How it Works

1. Load cadastral polygons (`cadastre.gpkg`) and GNAF property points (`gnaf_prop.parquet`).
2. Combine multiple address columns into `full_address`.
3. Define a function to calculate orientation based on the first edge of a polygon.
4. Apply the function to polygons, including MultiPolygons.
5. Convert property points to a GeoDataFrame and ensure CRS matches polygons.
6. Use a spatial join (`intersects`) to assign orientation from polygons to points.
7. Export the final CSV.

## Usage

```python
# Run the script in Python environment
python property_orientation_analysis.py
```

Check the console for a quick view of orientation counts. The final CSV will be saved as `property_orientation.csv`.

## Notes

* `polygon_orientation_simple` approximates the front of a house using the first polygon edge.
* MultiPolygons are handled by picking the biggest polygon.
* `intersects` is used for the spatial join to include points near polygon edges.
* The project works with lat/lon coordinates (EPSG:4326).

## Output

* CSV with two columns:

  * `full_address`: combined address of the property
  * `orientation`: compass direction the property faces (N, NE, E, SE, S, SW, W, NW)

## Findings (Simple Explanation)

I looked at about 68,000 properties. Most houses face **South, South-West, or West**, and very few face **North or North-East**. Homes in this area mostly go the southern way.

For investors:

* North-facing houses are rarer and might be more desirable.
* South-facing houses are common, so they may be standard in appeal.
* Knowing the direction helps think about sunlight, gardens, or solar panels.

The results aren’t perfect because some points didn’t match polygons exactly, but it still gives a good idea of which way most houses face.

## Optional Chart

You can make a bar chart to show counts of each orientation:

```python
import matplotlib.pyplot as plt

counts = {
    'S': 3884, 'SW': 1660, 'W': 1560, 'E': 700,
    'SE': 559, 'NW': 202, 'N': 183, 'NE': 146
}

plt.bar(counts.keys(), counts.values())
plt.title("Property Orientation Counts")
plt.xlabel("Orientation")
plt.ylabel("Number of Properties")
plt.show()
```
