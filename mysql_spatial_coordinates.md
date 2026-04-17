# MySQL Spatial Coordinates Rules

When working with MySQL spatial data types (such as `POINT`, `POLYGON`) and the GPS coordinate system (`SRID 4326`), you must strictly adhere to the following rules to avoid axis-order confusion.

## The Root Cause of Axis-Order Confusion
In standard Cartesian geometry and many web mapping APIs (like GeoJSON), coordinates are ordered `(X, Y)` which intuitively translates to `(Longitude, Latitude)`. However, MySQL 8 strictly implements the EPSG registry's definitions for Spatial Reference Systems. **For SRID 4326 (WGS 84), the official axis order is strictly (Latitude, Longitude).**

Because of this, MySQL maps the X-axis to Latitude and the Y-axis to Longitude for SRID 4326. This contradicts standard web GIS assumptions and is the primary reason developers and LLMs accidentally invert coordinate data.

## Strict Directives for SRID 4326

1. **Point Construction (`Point()`)**
   - When creating a Point explicitly for SRID 4326 using standard constructor functions, the arguments must be `Point(Latitude, Longitude)`.
   - **Incorrect:** `ST_SRID(Point(lng, lat), 4326)` (This will mistakenly store your Longitude value as the Latitude).
   - **Correct:** `ST_SRID(Point(lat, lng), 4326)`.

2. **Retrieving Coordinates (`ST_X` vs `ST_Latitude`)**
   - **NEVER** use `ST_X()` or `ST_Y()` to fetch coordinates when working with SRID 4326. `ST_X()` will return Latitude and `ST_Y()` will return Longitude, resulting in code that is highly unintuitive and prone to regression.
   - **ALWAYS** use the explicit alias functions `ST_Latitude()` and `ST_Longitude()`.
   - **Example:** `SELECT ST_Longitude(coordinates) AS lng, ST_Latitude(coordinates) AS lat`

3. **Well-Known Text (`ST_GeomFromText`)**
   - When parsing standard WKT (like `POLYGON(...)`) which is almost universally provided in `Lng Lat` order by frontend bounds arrays, you must explicitly instruct MySQL to override its default 4326 axis-order parsing.
   - **Correct:** `ST_GeomFromText(?, 4326, 'axis-order=long-lat')`
   - If you do not provide `'axis-order=long-lat'`, MySQL will parse standard WKT points as `(Lat Lng)` and will throw "Latitude out of range" errors if any longitude coordinate falls outside `[-90, 90]`.

4. **Bounding Boxes (`MBRContains`)**
   - Ensure that your WKT `POLYGON` string uses `Lng Lat` format coupled with the `'axis-order=long-lat'` parameter as shown above to avoid spatial inversion.
