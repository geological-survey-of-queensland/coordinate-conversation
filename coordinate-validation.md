# Validation of Coordinates

The ArcGIS REST Services Directory at https://gisservices.information.qld.gov.au/arcgis/rest/services provides access to services that can be used to identify or query spatial data.

* **Identify** is best if you know nothing but the intersect location and want to know what (if any) permits are under that point.
* **Query** is more difficult, and requires a more config to be hosted on your end, but is more precise. It is great to use if you know the intersect location, the Permit Type and the Permit Status. If you don’t know the Permit Type or Status, you could still use it, but it would require 20-40 requests to evaluate the intersect, which would take up to a minute to complete.

In this guide, we will use identify.

## Permit geometries

* Current mining permits are at: https://gisservices.information.qld.gov.au/arcgis/rest/services/Economy/MinesPermitsCurrent/MapServer
* Historic mining permits are at: https://gisservices.information.qld.gov.au/arcgis/rest/services/Economy/MinesPermitsHistoric/MapServer

Clicking the URLs above gives you all the layer IDs for the permit service for Mines. We are using collections of layers in the requests below based on permit purpose (exploration, production etc). This can be changed by using different IDs in the requests.

NOTE: The SIR data that these services run from is “next business day” for currency when compared to MMOL.  
ITP host an internal-only service running directly off the MMOL database that is real time currency.  
Next business day currency is good enough for most needs.

## HTML view of the service

To view the service in a HTML form, go to https://gisservices.information.qld.gov.au/arcgis/rest/services/Economy/MinesPermitsCurrent/MapServer/identify

## Making a request

1.	Construct a query to https://gisservices.information.qld.gov.au/arcgis/rest/services/Economy/MinesPermitsCurrent/MapServer/identify?
    1. Pass the point geometry as entered by the user in the user in the webform as longitude, latitude. in the format: 149.733566, -26.441947 - encoded this is `geometry=149.733566%2C+-26.441947`
    2. Geometry type will be a point - `geometryType=esriGeometryPoint`
    3. The spatial reference system ID (srid) is 4283 (GDA94). In the near future when they will change to GDA2020 which will change the srid to 7844. So, for now, pass `sr=4283`
    4. There is a layer for each type of permit. We want to query Exploration Permits (0) and Production Permits (30) (NOTE: we will test the performance and if too slow, then we will narrow the permit layers down). So pass layers=All:0,30 - encoded this is `layers=All%3A0%2C30`
    5. Set the tolerance to 1 - `tolerance=1`
    6. Create a map extent (a bounding box) using the geometry used in Step 2 to limit the extents of the spatial query. This is bottom-left longitude, latitude coordinate pair and top-right longitude, latitude coordinate pair, e.g. 149.73,-26.45,149.74,-26.44. Trim the long-lat entered by the user to 2 decimal places for the first coordinate pair, add .01 to the second coordinate pair. Encoded this is `mapExtent=149.73%2C-26.45%2C149.74%2C-26.44`
    7. Set imageDisplay=1000,1000 - encoded this is `imageDisplay=1000%2C1000`
    8. Set returnGeometry to true - `returnGeometry=true`
    9. Set the output format to JSON - `f=json`
2.	Send the query.
3.	In the JSON response:
    1. Check if the DISPLAYNAME attribute has a value equal to the permit number. e.g. `DISPLAYNAME: "PL 404"`.
    2. A true means that the point geometry did intersect the permit PL 404.
    3. A false means that the point geometry did not intersect the permit PL 404.

Try pasting this query below to get a JSON response that will meet a true test for PL 404:
https://gisservices.information.qld.gov.au/arcgis/rest/services/Economy/MinesPermitsCurrent/MapServer/identify?geometry=149.5602778%2C+-26.36527778&geometryType=esriGeometryPoint&sr=4283&layers=All%3A0%2C30&tolerance=1&mapExtent=149.73%2C-26.45%2C149.74%2C-26.44&imageDisplay=1000%2C1000&returnGeometry=true&f=json

## Example requests

Example requests, with HTML as the output format - to assist in understanding the API.

### TEST: Identify ATP 972 - positive result

**REQUEST SCOPE:** all CURRENT exploration (ID 0), production (ID 30), Infrastructure (ID 56), Information (ID 75) and Permit Admin Areas (ID81)

https://gisservices.information.qld.gov.au/arcgis/rest/services/Economy/MinesPermitsCurrent/MapServer/identify?geometry=149.7335667%2C+-26.44194722&geometryType=esriGeometryPoint&sr=4283&layers=All%3A0%2C30%2C56%2C75%2C81&tolerance=1&mapExtent=149.73%2C-26.45%2C149.74%2C-26.44&imageDisplay=1000%2C1000&returnGeometry=true&f=html 

### TEST: Identify ATP 972 - negative result

**REQUEST SCOPE:** all CURRENT exploration (ID 0), production (ID 30), Infrastructure (ID 56), Information (ID 75) and Permit Admin Areas (ID81)

https://gisservices.information.qld.gov.au/arcgis/rest/services/Economy/MinesPermitsCurrent/MapServer/identify?geometry=149.5602778%2C+-26.36527778&geometryType=esriGeometryPoint&sr=4283&layers=All%3A0%2C30%2C56%2C75%2C81&tolerance=1&mapExtent=149.73%2C-26.45%2C149.74%2C-26.44&imageDisplay=1000%2C1000&returnGeometry=true&f=html 

### TEST: Identify Positive for ATP 972 - improved performance

Improve performance by reducing the scope to just exploration and production permits

**REQUEST SCOPE:** CURRENT exploration (ID 0) and production (ID 30) permits only

https://gisservices.information.qld.gov.au/arcgis/rest/services/Economy/MinesPermitsCurrent/MapServer/identify?geometry=149.7335667%2C+-26.44194722&geometryType=esriGeometryPoint&sr=4283&layers=All%3A0%2C30&tolerance=1&mapExtent=149.73%2C-26.45%2C149.74%2C-26.44&imageDisplay=1000%2C1000&returnGeometry=true&f=html 

TEST: Identify Negative for ATP 972
**REQUEST SCOPE:** CURRENT exploration (ID 0) and production (ID 30) permits only

https://gisservices.information.qld.gov.au/arcgis/rest/services/Economy/MinesPermitsCurrent/MapServer/identify?geometry=149.5602778%2C+-26.36527778&geometryType=esriGeometryPoint&sr=4283&layers=All%3A0%2C30&tolerance=1&mapExtent=149.73%2C-26.45%2C149.74%2C-26.44&imageDisplay=1000%2C1000&returnGeometry=true&f=html 

## JSON responses

Change the requests to output json (for system use) by changing the last param to ‘f=json’

### TEST: Identify Positive for ATP 972 - JSON output

**REQUEST SCOPE:** all CURRENT exploration (ID 0), production (ID 30), Infrastructure (ID 56), Information (ID 75) and Permit Admin Areas (ID81)

https://gisservices.information.qld.gov.au/arcgis/rest/services/Economy/MinesPermitsCurrent/MapServer/identify?geometry=149.7335667%2C+-26.44194722&geometryType=esriGeometryPoint&sr=4283&layers=All%3A0%2C30%2C56%2C75%2C81&tolerance=1&mapExtent=149.73%2C-26.45%2C149.74%2C-26.44&imageDisplay=1000%2C1000&returnGeometry=true&f=json 

### TEST: Identify Negative for ATP 972 - JSON output

**REQUEST SCOPE:** all CURRENT exploration (ID 0), production (ID 30), Infrastructure (ID 56), Information (ID 75) and Permit Admin Areas (ID81)

https://gisservices.information.qld.gov.au/arcgis/rest/services/Economy/MinesPermitsCurrent/MapServer/identify?geometry=149.5602778%2C+-26.36527778&geometryType=esriGeometryPoint&sr=4283&layers=All%3A0%2C30%2C56%2C75%2C81&tolerance=1&mapExtent=149.73%2C-26.45%2C149.74%2C-26.44&imageDisplay=1000%2C1000&returnGeometry=true&f=json 

# Is the coordinate within Queensland?

Use the Coastline and State border geometry at:

https://gisservices.information.qld.gov.au/arcgis/rest/services/Boundaries/AdminBoundariesFramework/MapServer/1

# Is this a valid permit?

Use this query to check if PL6 is a valid permit:

https://gisservices.information.qld.gov.au/arcgis/rest/services/Economy/MinesPermitsCurrent/MapServer/find?searchText=PL+6&contains=false&searchFields=DISPLAYNAME&sr=&layers=2%2C3%2C5%2C6%2C8%2C9%2C11%2C12%2C14%2C15%2C17%2C18%2C19%2C22%2C25%2C28%2C29%2C33%2C36%2C40%2C44%2C48%2C49%2C51%2C52%2C54%2C55%2C59%2C63%2C67%2C68%2C70%2C71%2C73%2C74%2C77%2C78%2C79&returnGeometry=true&f=json

*Author*:  
**David Crosswell**  
Enterprise Architect  
Cross-Lateral Enterprises   
<https://crosslateral.com.au>
