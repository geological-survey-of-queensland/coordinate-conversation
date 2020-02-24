## TL;DR:

1. Geometries are stored in the PostGIS database in its native spatial format (PostGIS is part of the PostgreSQL database).  
2. Spatial data is converted on ingestion into the database (done by PostGIS).  
3. PostGIS spatial data can be output as WKT, WKB, GML, KML, GeoJSON, and SVG formats (done by PostGIS).  
4. PostGIS stores extended metadata about the spatial including the CRS (SRS).  
5. Spatial data as lodged is stored as a data object in its original format (NOTE: The data object is not stored in the PostGIS database):  
    a. A shape file submitted to GSQ will be stored as a data object in S3.  
    b. A webform that captures eastings/northings/zone stores these values in the database.  
    c. A GeoJSON value entered in a form gets stored in the database.

## What is PostGIS?

* PostGIS is a spatial database.  
* Spatial databases store and manipulate spatial objects like any other object in the database.  
* PostGIS turns the PostgreSQL Database Management System into a spatial database by adding support for the three features: spatial types, indexes, and functions.

## What types of geometries does PostGIS store?

![](https://image.slidesharecdn.com/getting-150930203034-lva1-app6891/95/getting-started-with-postgis-8-638.jpg?cb=1443645143)

Image credit: https://image.slidesharecdn.com/getting-150930203034-lva1-app6891/95/getting-started-with-postgis-8-638.jpg?cb=1443645143

## How is the geometry data arranged?

geometry_hierarchy.png

## What about 3D?

* PostGIS supports additional dimensions on all geometry types, a “Z” dimension to add height information and a “M” dimension for additional dimensional information (commonly time, or road-mile, or upstream-distance information) for each coordinate.

* For 3-D and 4-D geometries, the extra dimensions are added as extra coordinates for each vertex in the geometry, and the geometry type is enhanced to indicate how to interpret the extra dimensions.  
* Adding the extra dimensions results in three extra possible geometry types for each geometry primitive:  
    * Point (a 2-D type)i is joined by PointZ, PointM and PointZM types.  
    * Linestring (a 2-D type) is joined by LinestringZ, LinestringM and LinestringZM types.  
    * Polygon (a 2-D type) is joined by PolygonZ, PolygonM and PolygonZM types.  
    * And so on.  

## What about Raster?

PostGIS uses the raster type for storing and analysing raster data.

# Why do we want to store the geometries in PostGIS?

PostGIS gives us heaps of cool spatial functions. The majority of all spatial functions can be grouped into one of the following five categories:

* **Conversion:** Functions that convert between geometries and external data formats.
* **Retrieval:** Functions that retrieve properties and measurements of a Geometry.
* **Comparison:** Functions that compare two geometries with respect to their spatial relation.
* **Generation:** Functions that generate new geometries from others.
* **Management:** Functions that manage information about spatial tables and PostGIS administration.

## What about the Coordinate Reference System (CRS)/Spatial Reference System (SRS) (those two are synonyms)

* PostGIS provides two tables to track and report on the geometry types available in a given database.

* The first table, spatial_ref_sys, defines all the spatial reference systems known to the database (this is the SRID)
  * An “SRID” stands for “Spatial Reference IDentifier.” It defines all the parameters of our data’s geographic coordinate system and projection. An SRID is convenient because it packs all the information about a map projection (which can be quite complex) into a single number.
  * Every SRID has an EPSG identifier: e.g. https://spatialreference.org/ref/epsg/3112/

* The second table (actually, a view), geometry_columns, provides a listing of all “features” (defined as an object with geometric attributes), and the basic details of those features.

images/table01.png

## What about Geometry Input and Output

* Within the database, geometries are stored on disk in a format only used by the PostGIS program.

* In order for external programs to insert and retrieve useful geometries, they need to be converted into a format that other applications can understand.

* Fortunately, PostGIS supports emitting and consuming geometries in a large number of formats:  
  * Well-known text (WKT)  
  * Well-known binary (WKB)  
  * Geographic Mark-up Language (GML)  
  * Keyhole Mark-up Language (KML)  
  * GeoJSON  
  * Scalable Vector Graphics (SVG)

## What about Shape file ingestion?

* The pgShapeLoader makes shape data usable in PostGIS by converting it from binary data into a series of SQL commands that are then run in the database to load the data.

## Ok, so what are these cool functions you’re telling us about?

[ST_Area](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_Area.html): Returns the area of the surface if it is a polygon or multi-polygon. For “geometry” type area is in SRID units. For “geography” area is in square meters.

[ST_AsText](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_AsText.html): Returns the Well-Known Text (WKT) representation of the geometry/geography without SRID metadata.

[ST_AsBinary](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_AsBinary.html): Returns the Well-Known Binary (WKB) representation of the geometry/geography without SRID meta data.

[ST_EndPoint](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_EndPoint.html): Returns the last point of a LINESTRING geometry as a POINT.

[ST_AsEWKB](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_AsEWKB.html): Returns the Well-Known Binary (WKB) representation of the geometry with SRID meta data.

[ST_AsEWKT](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_AsEWKT.html): Returns the Well-Known Text (WKT) representation of the geometry with SRID meta data.

[ST_AsGeoJSON](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_AsGeoJSON.html): Returns the geometry as a GeoJSON element.

[ST_AsGML](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_AsGML.html): Returns the geometry as a GML version 2 or 3 element.

[ST_AsKML](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_AsKML.html): Returns the geometry as a KML element. Several variants. Default version=2, default precision=15.

[ST_AsSVG](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_AsSVG.html): Returns a Geometry in SVG path data given a geometry or geography object.

[ST_ExteriorRing](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_ExteriorRing.html): Returns a line string representing the exterior ring of the POLYGON geometry. Return NULL if the geometry is not a polygon. Will not work with MULTIPOLYGON

[ST_GeometryN](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_GeometryN.html): Returns the 1-based Nth geometry if the geometry is a GEOMETRYCOLLECTION, MULTIPOINT, MULTILINESTRING, MULTICURVE or MULTIPOLYGON. Otherwise, return NULL.

[ST_GeomFromGML](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_GeomFromGML.html): Takes as input GML representation of geometry and outputs a PostGIS geometry object.

[ST_GeomFromKML](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_GeomFromKML.html): Takes as input KML representation of geometry and outputs a PostGIS geometry object

[ST_GeomFromText](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_GeomFromText.html): Returns a specified ST_Geometry value from Well-Known Text representation (WKT).

[ST_GeomFromWKB](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_GeomFromWKB.html): Creates a geometry instance from a Well-Known Binary geometry representation (WKB) and optional SRID.

[ST_GeometryType](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_GeometryType.html): Returns the geometry type of the ST_Geometry value.

[ST_InteriorRingN](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_InteriorRingN.html): Returns the Nth interior linestring ring of the polygon geometry. Return NULL if the geometry is not a polygon or the given N is out of range.

[ST_Length](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_Length.html): Returns the 2d length of the geometry if it is a linestring or multilinestring. geometry are in units of spatial reference and geography are in meters (default spheroid)

[ST_NDims](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_NDims.html): Returns coordinate dimension of the geometry as a small int. Values are: 2,3 or 4.

[ST_NPoints](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_NPoints.html): Returns the number of points (vertexes) in a geometry.

[ST_NRings](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_NRings.html): If the geometry is a polygon or multi-polygon returns the number of rings.

[ST_NumGeometries](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_NumGeometries.html): If geometry is a GEOMETRYCOLLECTION (or MULTI*) returns the number of geometries, otherwise return NULL.

[ST_Perimeter](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_Perimeter.html): Returns the length measurement of the boundary of an ST_Surface or ST_MultiSurface value. (Polygon, Multipolygon)

[ST_SRID](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_SRID.html): Returns the spatial reference identifier for the ST_Geometry as defined in spatial_ref_sys table.

[ST_StartPoint](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_StartPoint.html): Returns the first point of a LINESTRING geometry as a POINT.

[ST_X](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_X.html): Returns the X coordinate of the point, or NULL if not available. Input must be a point.

[ST_Y](https://mcas-proxyweb.us2.cas.ms/certificate-checker?login=false&originalUrl=http%3A%2F%2Fpostgis.net.us2.cas.ms%2Fdocs%2FST_Y.html): Returns the Y coordinate of the point, or NULL if not available. Input must be a point.

## I’ve never heard of PostGIS – are you pulling one on us?

* PostGIS was first released 19 April 2001  
* PostGIS has become a widely used spatial database – for example: Goldman Sachs, United Nations, Nokia, BMW  
* What tools support PostGIS? See here: https://trac.osgeo.org/postgis/wiki/UsersWikiToolsSupportPostgis  

## Need more info?

If you’re interested for more, read here: http://postgis.net/workshops/postgis-intro/index.html

## Examples

Want to see some examples: https://tjukanov.org/
