## Introduction


GSQ receive co-ordinates in paper forms from industry in the following formats:
- GDA94 - Lat-Long Degrees, Minutes, Decimal Seconds (L/L)
- MGA94 - Eastings, Nothings, Zone number (in metres) (E/N)

GSQ receive co-ordinates from industry in the following formats:
* GDA94 - Latitude, Longitude in Degrees, Minutes, Decimal Seconds
* MGA94 - Eastings, Nothings, Zone number (in metres)

## GDA2020


See [Geocentric Datum of Australia (GDA)](https://www.icsm.gov.au/australian-geospatial-reference-system)


## Spatial coordinate data flow
<p align="center">
<img src="https://github.com/geological-survey-of-queensland/coordinate-conversion/blob/master/images/spatial_coordinate-workflow.svg" width="400"><br>
Figure 1: Spatial coordinate data flow</p>

## Coordinate Input Forms
The coordinate input forms must cater for the three input options:
* Latitude, longitde decimal
* Latitude, longitde degrees, minutes, seconds
* Eastings, northings, zone

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/coordinate-conversion/blob/master/images/coordinate_input_form.png" width="680"><br>
Figure 2: Example spatial coordinate input form</p>

## Coordinate Conversion
http://appsuppt103/confluence/display/1EP/UI+component+-+latitude+longitude+input


## Coordinate Validation


## Coordinate Database Storage
ISO 6709 *Standard representation of geographic point location by coordinates* is the international standard for representation of latitude, longitude and altitude for geographic point locations. The standard states:
* Fraction of degrees is preferred in digital data exchange, while sexagesimal notation is tolerated for compatibility.

### Retention of originally entered coordinates
The database is to store the spatial coordinates and CRS as entered by the user or entered programatically. This data is for reference only and is not used for spatial display.

## Coordinate Display
ISO 6709 suggests the following for representation at the human interface :

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

Examples:  
* 50°40′46.461″N 95°48′26.533″W 123,45m  
* 50°03′46.461″S 125°48′26.533″E 978.90m

## As is functionality
* The user manually types in the information provided in the paper form into MERLIN in either L/L or E/N.
* The data is stored in two separate field sets in the database.
* When one set of co-ordinates is entered, the other coordinates are nulled by the MERLIN UI.
* There is a nightly process which identifies any nulled coordinates and attempts to populate them with a conversion on the other coordinates.  E.g.  if L/L is populated, E/N is nulled, nightly process recognises E/N is null and calculates the value from L/L.
* The current conversion is a python script using the ArcPy library (http://pro.arcgis.com/en/pro-app/arcpy/get-started/what-is-arcpy-.htm). ArcPy is an ESRI product (http://www.esri.com/)

## To be functionality
- The user manually types in the information provided in the paper form into GEM in either L/L or E/N.
- The data is converted into Lat-Long Decimal Degrees when saved to the database.
- When data is retrieved from the database, it converts from Lat-Long decimal degrees into L/L & E/N on the UI.
- There are no nightly processes to monitor and maintain anymore.
- The current conversion tool in use is the proj.4 library provided by the Open Source Geospatial Foundation (http://www.osgeo.org/), which uses the GDA94 technical manual version 2.4 calculation (http://www.icsm.gov.au/gda/tech.html).
- The reason this software was chosen is becuase it fits with our current technology stack, is simple and lightweight and is a mature project which uses the GDA94 technical manual defined by ICSM.
- ArcPy is a python library which does not fit with our current technology stack.

## Possible option
- There is an enhancement to change the conversion tool to the GDAy tool (https://www.business.qld.gov.au/industry/titles-property-construction/surveying/gda-transformation-software).
- GDAy also uses the ICSM technical manual for calculations.


## You want how many decimal places??
Sometimes we get a little carried away with the number of decimal places to which we store coordinates in a database (6 is plenty for us).

|Decimal Places|Aprox. Distance|Say What?|
|---|---|---|
|1|10 kilometers|6.2 miles|
|2|1 kilometer|0.62 miles|
|3|100 meters|About 328 feet|
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
