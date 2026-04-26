<agent_rules>
# Pike County GIS API Integration Rules

This rule file defines the technical specifications and interaction patterns for accessing the Pike County, Pennsylvania GIS data via its ArcGIS REST API. When developing applications or writing scripts that interface with this data, follow these guidelines strictly.

## Overview
The Pike County, PA Geospatial Information System (GIS) is built on ArcGIS Server technology. You should interact directly with the REST API endpoints to retrieve structured parcel and ownership data, rather than scraping web interfaces.

## API Endpoints
- **Base Services Directory**: `https://gis.pikepa.org/arcgis/rest/services/`
- **Primary Parcel Layer**: `https://gis.pikepa.org/arcgis/rest/services/PikeCo_Parcels/MapServer/0`
- **Query Endpoint**: `https://gis.pikepa.org/arcgis/rest/services/PikeCo_Parcels/MapServer/0/query`

## Search Logic & Parameters
To perform lookups by street address, construct `GET` or `POST` requests to the **Query Endpoint** using the following logic:

### 1. The `where` Clause
The default property data is stored in uppercase. To ensure searches are case-insensitive, always use the SQL `UPPER()` function combined with the `LIKE` operator and wildcards (`%`). 
- **Standard Field**: `Situs` (The physical address on record)
- **Syntax**: `where=UPPER(Situs) LIKE UPPER('%[ADDRESS_STRING]%')`
- **Street Names**: Please use abbreviations when searching by the road name.  For example, use "St" instead of Street.

### 2. Output Formatting
- **Format (`f`)**: Always request `json` or `pjson` for machine readability.
- **Fields (`outFields`)**: Request specific fields to minimize payload. *Do not use incorrect field names like Owner1 or Situs_Address*. The valid key fields are:
  - `Name`: The registered property owner(s).
  - `Situs`: The physical address on record.
  - `MAP_JOIN`: The parcel ID / map join identifier.
  - `Control`: The control number for the property.
  - `Addr1`, `Addr2`, `City`, `State`, `Zipcode`: The owner's mailing/billing address.
- **Geometry**: Set `returnGeometry=false` for simple text lookups to improve response speed and avoid large payloads.

## Interaction Example
To look up the owner of a property on "Broad St", structure the query as follows (make sure to properly URL encode values in actual implementations, e.g., `%25` for `%`):

```http
GET /arcgis/rest/services/PikeCo_Parcels/MapServer/0/query?where=UPPER(Situs)+LIKE+UPPER('%25BROAD%25')&outFields=Name,Situs,Control,MAP_JOIN&f=pjson&returnGeometry=false HTTP/1.1
Host: gis.pikepa.org
```

## Error Handling & Rate Limiting
- **No Results**: If a query returns an empty `features` array, attempt to generalize the search (e.g., search only for the street name and number without the suffix, like `"123 MAIN"` instead of `"123 MAIN ST"`).
- **Case Sensitivity**: As noted above, failure to use `UPPER()` might result in empty returns if the casing doesn't exactly match the uppercase records in the database.
- **URL Encoding**: Ensure all spaces and special characters in the query string are properly percent-encoded (e.g., `%20` or `+` for space, `%25` for the `%` wildcard, `%27` for single quotes).
- **Timeouts**: Large spatial queries may time out. For address-only lookups, ensure `returnGeometry=false` is set.

## Documentation & Resources
- **ArcGIS REST API Documentation**: Refer to the [ESRI Query Reference](https://developers.arcgis.com/rest/services-reference/enterprise/query-map-service-layer-.htm) for advanced filtering (e.g., spatial intersections or date-based queries).
- **Machine-Readable API Discovery**: The ArcGIS REST API is inherently self-documenting. Appending `?f=pjson` to *any* endpoint (the base services directory, a specific MapServer, or a layer) returns a machine-readable JSON representation defining all available layers, operations, spatial references, and field metadata.
</agent_rules>
