Property Orientation Analysis
This project calculates the facing direction of residential properties in Sydney suburbs and combines full addresses from separate components. It produces a CSV where each row represents one property, showing its address and orientation.
Features
Calculates property orientation from cadastral polygons.
Handles MultiPolygons by using the largest polygon.
Combines GNAF address fields into a single readable full_address.
Spatially joins property points to their polygons.
Outputs a CSV with full_address and orientation for easy use by investors or analysts.
How it Works
Load cadastral polygons (cadastre.gpkg) and GNAF property points (gnaf_prop.parquet).
Combine multiple address columns into full_address.
Define a function to calculate orientation based on the first edge of a polygon.
Apply the function to polygons, including MultiPolygons.
Convert property points to a GeoDataFrame and ensure CRS matches polygons.
Use a spatial join (intersects) to assign orientation from polygons to points.
Export the final CSV.
Usage
# Run the script in Python environment
python property_orientation_analysis.py
Check the console for a quick view of orientation counts. The final CSV will be saved as property_orientation.csv.
Notes
polygon_orientation_simple approximates the front of a house using the first polygon edge.
MultiPolygons are handled by picking the biggest polygon.
intersects is used for the spatial join to include points near polygon edges.
The project works with lat/lon coordinates (EPSG:4326).
Output
CSV with two columns:
full_address: combined address of the property
orientation: compass direction the property faces (N, NE, E, SE, S, SW, W, NW)
Findings (Simple Explanation)
I looked at about 74,547 properties. Only 8,894 properties got an orientation, and 65,653 are missing (NaN). This happened because many property points didn’t match a polygon properly or the geometry wasn’t ready for orientation calculation.
For investors:
Among the properties we could check, most houses face South, South-West, or West.
North-facing houses are rare, so they might be more desirable.
Even with missing data, the trends give a rough idea about which way houses face.
This shows our method works for some properties but could be improved with better polygon matching, buffers, or nearest neighbor assignment.
