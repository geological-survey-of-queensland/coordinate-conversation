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

You must supply the following to make the request:

* The point geometry for the intersect
* The layer IDs to get results from - examples below
* The map extent - you’ll need to calculate this to be a bbox around the point geometry, you’ll need to specify at least 2 decimal places to get 1m intersect accuracy, 3 decimal places will get you 10cm. Mining permits are pretty big so I stuck with 1m in my examples
* The tolerance in pixels - just use a value 1 for your purposes
* The image display size in pixels - leave this at 1000,1000, this determines the accuracy of the intersect, when combined with the map extent. You could double to 2000 and increase accuracy without changing bbox, but there is a performance trade off here.

When checking if a point is within a permit boundary, you must know the following to evaluate the result:

* The DISPLAYNAME value of the desired permit to be checked eg “ATP 972”

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
