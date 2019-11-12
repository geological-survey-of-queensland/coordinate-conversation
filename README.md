## Introduction
GSQ receive co-ordinates in paper forms from industry in the following formats:
- GDA94 - Lat-Long Degrees, Minutes, Decimal Seconds (L/L)
- MGA94 - Eastings, Nothings, Zone number (in metres) (E/N)

GSQ receive co-ordinates from industry in the following formats:
* GDA94 - Latitude, Longitude in Degrees, Minutes, Decimal Seconds
* MGA94 - Eastings, Nothings, Zone number (in metres)

Both are valid geospatial 

ISO 6709:2011 

See [Geocentric Datum of Australia (GDA)](https://www.icsm.gov.au/australian-geospatial-reference-system)

http://appsuppt103/confluence/display/1EP/UI+component+-+latitude+longitude+input
 
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
