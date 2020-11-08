# GeoJSON

* [GeoJSON](https://tools.ietf.org/html/rfc7946) is an open standard format designed for representing simple geographical features, along with their non-spatial attributes. It is based on the JavaScript Object Notation (JSON).  
* GeoJSON is widely used in JavaScript web-mapping libraries, JSON-based document databases, and web APIs, and became the de facto standard for web mapping when Google Maps adopted it in 2005.
* GeoJSON uses the World Geodetic System 1984 (WGS 84) coordinate reference system, with longitude and latitude units of decimal degrees.
* GSQ uses GeoJSON in its CKAN data catalogue to show the spatial representation of a dataset.
* The difference between GDA2020 and WGS84 coordinates was approximately 21 cm in January 2017, approximately 0 cm in January 2020, then 21 cm in 2023, and then continue to diverge by approximately 7cm per year. As GeoJSON is used as a representation of the geometry, not the precise geometry, this difference is not noticeable to the web user.

#### GeoJSON bounding box example

Longitude then latitude, start at bottom-left then go counter-clockwise, finishing back at bottom-left.

```javascript
{
    "type": "Polygon",
    "coordinates": [
        [
            [148.0011, -26.9984],
            [150.4177, -26.9984],
            [150.4177, -24.1650],
            [148.0011, -24.1650],
            [148.0011, -26.9984]
        ]
    ]
}
```

#### GeoJSON point example
```javascript
{
    "type": "Point",
    "coordinates": [149.0263917, -26.7495441]
}
```

From the The GeoJSON Format RFC https://tools.ietf.org/html/rfc7946

# Appendix A. Geometry Examples

Each of the examples below represents a valid and complete GeoJSON object.

## A.1. Points

Point coordinates are in x, y order (easting, northing for projected coordinates, longitude, and latitude for geographic coordinates):

```javascript
     {
         "type": "Point",
         "coordinates": [100.0, 0.0]
     }
```

## A.2. LineStrings

Coordinates of LineString are an array of positions (see Section 3.1.1):
   
```javascript
     {
         "type": "LineString",
         "coordinates": [
             [100.0, 0.0],
             [101.0, 1.0]
         ]
     }
```     
     
## A.3. Polygons

Coordinates of a Polygon are an array of linear ring (see Section 3.1.6) coordinate arrays.  The first element in the array represents the exterior ring.  Any subsequent elements represent interior rings (or holes).

### No holes:
```javascript
     {
         "type": "Polygon",
         "coordinates": [
             [
                 [100.0, 0.0],
                 [101.0, 0.0],
                 [101.0, 1.0],
                 [100.0, 1.0],
                 [100.0, 0.0]
             ]
         ]
     }
```

### With holes:

```javascript
     {
         "type": "Polygon",
         "coordinates": [
             [
                 [100.0, 0.0],
                 [101.0, 0.0],
                 [101.0, 1.0],
                 [100.0, 1.0],
                 [100.0, 0.0]
             ],
             [
                 [100.8, 0.8],
                 [100.8, 0.2],
                 [100.2, 0.2],
                 [100.2, 0.8],
                 [100.8, 0.8]
             ]
         ]
     }
```  
     
## A.4. MultiPoints
Coordinates of a MultiPoint are an array of positions:

```javascript
     {
         "type": "MultiPoint",
         "coordinates": [
             [100.0, 0.0],
             [101.0, 1.0]
         ]
     }
```

## A.5. MultiLineStrings

Coordinates of a MultiLineString are an array of LineString coordinate arrays:

```javascript
     {
         "type": "MultiLineString",
         "coordinates": [
             [
                 [100.0, 0.0],
                 [101.0, 1.0]
             ],
             [
                 [102.0, 2.0],
                 [103.0, 3.0]
             ]
         ]
     }
```

## A.6. MultiPolygons

Coordinates of a MultiPolygon are an array of Polygon coordinate arrays:

```javascript
     {
         "type": "MultiPolygon",
         "coordinates": [
             [
                 [
                     [102.0, 2.0],
                     [103.0, 2.0],
                     [103.0, 3.0],
                     [102.0, 3.0],
                     [102.0, 2.0]
                 ]
             ],
             [
                 [
                     [100.0, 0.0],
                     [101.0, 0.0],
                     [101.0, 1.0],
                     [100.0, 1.0],
                     [100.0, 0.0]
                 ],
                 [
                     [100.2, 0.2],
                     [100.2, 0.8],
                     [100.8, 0.8],
                     [100.8, 0.2],
                     [100.2, 0.2]
                 ]
             ]
         ]
     }
```

## A.7. Geometry Collections

Each element in the "geometries" array of a GeometryCollection is one of the Geometry objects described above:

```javascript
     {
         "type": "GeometryCollection",
         "geometries": [{
             "type": "Point",
             "coordinates": [100.0, 0.0]
         }, {
             "type": "LineString",
             "coordinates": [
                 [101.0, 0.0],
                 [102.0, 1.0]
             ]
         }]
     }
```
