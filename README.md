## Introduction

This guide recommends the capture, storage, and display of geometric representations of GSQ data.
<p align="center">
<img src="https://github.com/geological-survey-of-queensland/spatial-coordinate-handling/blob/master/images/spatial-coordinate-handling-overview.png"><br>
Figure 1: Lifecycle of geometric representation</p>

## TL;DR

**What is a datum?**  
A datum provides a frame of reference for measuring locations on the surface of the earth.

**What is a projection?**  
Map projections are mathematical formulae which allow areas on the surface of the Earth (a
spheroid) to be represented on a map (a flat surface).

**What is a coordinate system?**  
A reference system used to measure horizontal and vertical distances on a planimetric map. It
is used to locate x,y positions of point, line, and area features.

**What datum is used in Australia?**  
The Geocentric Datum of Australia (GDA) is the current datum for all of Australia. It was
first implemented in 1994 (GDA94).  
Because, in 2020, the continent of Australia will have moved 1.8 metres north-east of where it was in 1994, GDA2020 will replace GDA94.

**What is WGS84**  
WGS84 is the World Geodetic System 1984. It provides the current standard for locational
measurement worldwide, particularly in conjunction with the Global Positioning System
(GPS) satellite network. Data collected in WGS84 is directly compatible with that collected in
GDA94.

**What do AGD, AMG, GDA and MGA stand for?**  

* **AGD** stands for the Australian Geodetic Datum. It was produced in two versions, 1966 and
1984 (hence reference to AGD66 and AGD84), and was the standard datum used by Australia
up to 1994. There is no significant difference between AGD66 and AGD84.  
* **AMG** is the Australian Map Grid, as applied to either AGD66 or AGD84, in a projected
coordinate system (as opposed to a geographic coordinate system). AMG uses Cartesian
coordinates (easting, northings) rather than latitude/ longitude.  
* **GDA** is the Geocentric Datum of Australia. It has been implemented as the standard datum
since 1994, replacing AGD66/84. All data in Australia should be now be recorded using a
GDA coordinate system (either projected, or geographic coordinates).  
* **MGA** is the Map Grid of Australia as applied to GDA94 and GDA2020, in a projected coordinate system (as
opposed to a geographic coordinate system). MGA uses Cartesian coordinates (easting,
northings) rather than latitude/ longitude. Brisbane falls with MGA grid zone 56,
whilst Mt Isa lies in MGA zone 54. See http://www.icsm.gov.au/datum/gda-frequently-asked-questions

## Background

### ISO 6709

* ISO 6709 _Standard representation of geographic point location by coordinates_ is the international standard for representation of latitude, longitude and altitude for geographic point locations.
* Under ISO 6709, a geographical point is specified by the following four items:
  * First horizontal coordinate (y), such as latitude (negative number south of equator and positive north of equator)
  * Second horizontal coordinate (x), such as longitude (negative values west of Prime Meridian and positive values east of Prime Meridian)
  * Vertical coordinate, i.e. height or depth (optional)
  * Identification of coordinate reference system (CRS) (optional)

### Coordinate reference system (CRS)

* A coordinate reference system or spatial reference system (SRS) is a coordinate-based local, regional or global system used to locate geographical entities.  
* Map projections try to portray the surface of the earth, or a portion of the earth, on a flat piece of paper or computer screen. Map projections try to transform the earth from its spherical shape (3D) to a planar shape (2D).  
* A coordinate reference system (CRS) then defines how the two-dimensional, projected map in your GIS relates to real places on the earth. The decision of which map projection and CRS to use depends on the regional extent of the area you want to work in, on the analysis you want to do, and often on the availability of data.

### GDA2020 CRS

* The datum Australia uses now is called the Geocentric Datum of Australia 1994, or GDA94.
* This grid of latitude and longitude coordinates moves with the drift of the continent, but satellite positioning systems base their coordinates on a framework that is fixed to the centre of the Earth.
* By 2020, Australia will have moved 1.8 metres north-east of where it was in 1994. The discrepancies between real-time, precise satellite positioning and GDA94 will be very noticeable.
* [GDA2020](https://www.icsm.gov.au/gda2020) defines a new datum 1.8 metres to the north-east of GDA94.
* Stage 2 of GDA2020 will establish a dynamic datum that will continually model Australia’s movement.

### PostGIS

* PostGIS is a spatial database extension of PostgreSQL.
* Spatial databases store and manipulate spatial objects using three features: spatial types, indexes, and functions.
* Geometries are stored in the PostGIS database in its native spatial format.
* Spatial data is converted on ingestion into the database (done by PostGIS).
* PostGIS spatial data can be output as WKT, WKB, GML, KML, GeoJSON, and SVG formats (done by PostGIS).
* PostGIS stores extended metadata about the spatial including the CRS (SRS) as a SRID.

### SRID

* A Spatial Reference System Identifier (SRID) is a unique value used to unambiguously identify projected, unprojected, and local spatial coordinate system definitions.
* PostGIS uses the EPSG Geodetic Parameter Dataset.
* EPSG SRID examples are:
  * Geographic 2D GDA2020 - [7844](https://epsg.io/7844)
  * Geographic 3D GDA2020 - [7843](https://epsg.io/7843)
  * WGS84 World Geodetic System 1984 - [4326](https://epsg.io/4326)
  * GDA94 geographic coordinate system - [4823](https://epsg.io/4283)
  * MGA Zone 52 GDA2020 - [7852](https://epsg.io/7852)
  * MGA Zone 53 GDA2020 - [7853](https://epsg.io/7853)
  * MGA Zone 54 GDA2020 - [7854](https://epsg.io/7854)
  * MGA Zone 55 GDA2020 - [7855](https://epsg.io/7855)
  * MGA Zone 56 GDA2020 - [7856](https://epsg.io/7856)
  * MGA Zone 57 GDA2020 - [7857](https://epsg.io/7857)

PostGIS uses the SRID when inserting a geometry:  
`ST_GeomFromText(wkt, srid)`

For example:  
`INSERT INTO app(the_geom)
VALUES(ST_GeomFromText('POINT(-71.060316 48.432044)', 7844));`

### Coorindate transformation vs conversion

* Changing a coordinate on one datum (the source) to the corresponding coordinate on another datum (the target) involves a process called **coordinate transformation**.
* **Coordinate conversions** switch between coordinate reference systems on the same datum or reference frame, such as from a geographic coordinate (GDA2020 latitude and longitude) to a projected coordinate (MGA2020 Easting, Northing and Zone).

### GeoJSON

* [GeoJSON](https://tools.ietf.org/html/rfc7946) is an open standard format designed for representing simple geographical features, along with their non-spatial attributes. It is based on the JavaScript Object Notation (JSON).  
* GeoJSON is widely used in JavaScript web-mapping libraries, JSON-based document databases, and web APIs, and became the de facto standard for web mapping when Google Maps adopted it in 2005.
* GeoJSON uses the World Geodetic System 1984 (WGS 84) coordinate reference system, with longitude and latitude units of decimal degrees.
* GSQ uses GeoJSON in its CKAN data catalogue to show the spatial representation of a dataset.
* The difference between GDA2020 and WGS84 coordinates was approximately 21 cm in January 2017, approximately 0 cm in January 2020, then 21 cm in 2023, and then continue to diverge by approximately 7cm per year. As GeoJSON is used as a representation of the geometry, not the precise geometry, this difference is not noticeable to the web user.

## Where can I get spatial data about mining permits?

* Current mining permits are at: https://gisservices.information.qld.gov.au/arcgis/rest/services/Economy/MinesPermitsCurrent/MapServer
* Historic mining permits are at: https://gisservices.information.qld.gov.au/arcgis/rest/services/Economy/MinesPermitsHistoric/MapServer

See this [dedicated page](https://github.com/geological-survey-of-queensland/spatial-coordinate-handling/blob/master/coordinate-validation.md) on coordinate validation.

## How does industry submit spatial geometry to GSQ?

GSQ receives co-ordinates from industry in the following formats:

* Latitude, longitde decimal
* Latitude, longitde degrees, minutes, seconds
* Eastings, northings, zone
* In binary format - e.g. a shapefile (ESRI .SHP format)

## Spatial coordinate workflow
<p align="center">
<img src="https://github.com/geological-survey-of-queensland/spatial-coordinate-handling/blob/master/images/spatial-coordinate-workflow.png" width="100%"><br>
Figure 2: Spatial coordinate workflow</p>

## What are the 3 different coordinate input forms?

### 1. Latitude, longitde decimal
<p align="center">
<kbd><img src="https://github.com/geological-survey-of-queensland/coordinate-conversion/blob/master/images/coordinate_input_form-lld.png" width="100%"></kbd><br>
Figure 3: Latitude, longitde decimal spatial coordinate input form</p>

### 2. Latitude, longitde degrees, minutes, seconds
<p align="center">
<kbd><img src="https://github.com/geological-survey-of-queensland/coordinate-conversion/blob/master/images/coordinate_input_form-dms.png" width="100%"></kbd><br>
Figure 4: Latitude, longitde degrees, minutes, seconds spatial coordinate input form</p>

### 3. Eastings, northings, zone
<p align="center">
<kbd><img src="https://github.com/geological-survey-of-queensland/coordinate-conversion/blob/master/images/coordinate_input_form-enz.png" width="100%"></kbd><br>
Figure 5: Eastings, northings, zone spatial coordinate input form</p>

## Coordinate Transformation

Coordinate transformation is the process of changing coordinates from one reference frame or datum to another: for example, from GDA94 to GDA2020 or from GDA2020 to ITRF 2014.

In contrast, a coordinate conversion changes coordinates from one coordinate reference system to another coordinate reference system on the same datum or reference frame: for example, from MGA2020 Easting, Northing and Zone to GDA2020 latitude and longitude.

* See [GDA Transformation products and tools](https://www.icsm.gov.au/datum/gda-transformation-products-and-tools)  
* See [GDA94 ⇔ GDA2020 transformation and conversion tools](https://www.icsm.gov.au/datum/gda-transformation-products-and-tools/software-and-plugins)  
* See [Geocentric Datum of Australia 2020 Technical Manual](https://www.icsm.gov.au/sites/default/files/GDA2020TechnicalManualV1.1.1.pdf)

NOTE: ICSM has not defined a set of parameters that directly transform between AGD66 ⁄ AGD84 and GDA2020. It is recommended to first transform to GDA94 and then to GDA2020.

## Coordinate Conversion

Todo

## Coordinate Validation

See this [dedicated page](https://github.com/geological-survey-of-queensland/spatial-coordinate-handling/blob/master/coordinate-validation.md) on coordinate validation.

## Storing geometry in PostGIS database

1. Geometries are stored in the PostGIS database in its native spatial format (PostGIS is part of the PostgreSQL database).
1. The source of truth for geometry is the spatial object stored in PostGIS.
1. Spatial data is transformed and/or converted on ingestion into PostGIS.
1. The spatial object is to be stored in GDA202, i.e. the SRID = 7844.

## Retention of originally entered coordinates

The database is to store the spatial coordinates and CRS as entered by the user or entered programatically. This data is for reference only and is not used for spatial display.

* A shape file submitted to GSQ will be stored as a data object in S3.
* A webform that captures `latitude, longitude, CRS` stores these values in the database.
* A webform that captures `latitude, longitde degrees, minutes, seconds, CRS` stores these values in the database.
* A webform that captures `eastings, northings, zone, CRS` stores these values in the database.
* A GeoJSON value entered in a form gets stored in the database as GeoJSON.

### Storing coordinates in PostgreSQL database - ISO 6709 based

ISO 6709 *Standard representation of geographic point location by coordinates* is the international standard for representation of latitude, longitude and altitude for geographic point locations. The standard states:

* Fraction of degrees is preferred in digital data exchange.
* Data type for fractions of degrees is Float (9,6).
* However, because we are storing the values for reference only, the database columns will be:
  * TBC
  * TBC

## Coordinate Display - ISO 6709 based

ISO 6709 suggests the following for representation at the human interface:

1. Coordinate values (latitude, longitude, and altitude) should be delimited by spaces.
1. The decimal point is a part of the value, thus must usually be configured by the operating system.
1. Multiple points should be represented by multiple lines.
1. Latitude and longitude should be displayed by sexagesimal fractions (i.e. minutes and seconds).
1. When minutes and seconds are less than ten, leading zeroes should be shown.
1. Degree, minutes and seconds should be followed by the symbols ° (U+00B0), ′ (U+2032), and ″ (U+2033), without spaces between the number and symbol.
1. North and south latitudes should be indicated by N and S following immediately after the digits.
1. East and west longitudes should be indicated by E and W following immediately after the digits.
1. Units of elevation or depth should be given by symbols, immediately after the digits.
1. Elevation below reference level or depth above reference level should be indicated by a minus sign − (U+2212).

Sexagesimal Examples:  

* 50°40′46.461″N 95°48′26.533″W 123,45m  
* 50°03′46.461″S 125°48′26.533″E 978.90m

### Extension of ISO 6709

The standard has been extended to allow for depths and heights, and to include coordinate reference system identification.

Decimal examples:  

* Mount Everest +27.5916+086.5640+8850CRSWGS_84/
* South Pole -90+000+2800CRSWGS_84/

## You want how many decimal places?

Sometimes we get a little carried away with the number of decimal places to which we store coordinates in a database (6 is plenty for us).

|Decimal Places|Aprox. Distance|Say What?|
|---|---|---|
|1|10 kilometers|6.2 miles|
|2|1 kilometer|Length of Sydney Harbour bridge|
|3|100 meters|Usain Bolt ran this quite fast|
|4|10 meters|About 33 feet, the the limit of accuracy for a standard GPS device|
|5|1 meter|About 3 feet|
|6|10 centimeters|Length of a cigarette, the limit of accuracy for a differential GPS device|
|7|1.0 centimeter|Width of an average fingernail|
|8|1.0 millimeter|The width of paperclip wire|
|9|0.1 millimeter|The width of a strand of hair|
|10|10 microns|A speck of pollen|
|11|1.0 micron|A piece of cigarette smoke|
|12|0.1 micron|You're doing virus-level mapping at this point|
|13|10 nanometers|Thickness of a cell wall of a bacteria|
|14|1.0 nanometer|Your fingernail grows about this far in one second|
|15|0.1 nanometer|An atom. An atom! What are you mapping?|

## See also

* [Geocentric Datum of Australia (GDA)](https://www.icsm.gov.au/australian-geospatial-reference-system)
* [Guidelines for Transition to GDA2020 in SIR plus tools ver. 1.1](https://github.com/geological-survey-of-queensland/spatial-coordinate-handling/blob/master/files/Guidelines%20for%20Transition%20to%20GDA2020%20in%20SIR%20plus%20tools%20ver.%201.1.pdf)
* [Selecting the correct datum, coordinate system and projection for North Australian applications](https://www.environment.gov.au/system/files/resources/324e5fa5-e819-4cd6-b569-98aec2da62e7/files/ir473.pdf) - a very good guide to spatial coordinates in Australia.

## Licence

This code repository's content are licensed under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/), the deed of which is stored in this repository here: [LICENSE](LICENSE).

## Contacts

*System owner*:  
**Mark Gordon**  
Geological Survey of Queensland  
Department of Natural Resources, Mines and Energy  
Brisbane, QLD, Australia  
<mark.gordon@dnrme.qld.gov.au>  

*Author*:  
**David Crosswell**  
Enterprise Architect  
Cross-Lateral Enterprises  
<https://crosslateral.com.au>  
