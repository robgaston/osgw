title: Open-source Geospatial Web Stack Workshop
class: animation-fade
layout: true

<!-- This slide will serve as the base layout for all your slides -->
.bottom-bar[
  {{title}}
]

---

class: impact

# {{title}}, Day 2
## July 31, GIS Education Center, Galvanize

---

class: impact

# Day 1 Review

---

## What did we learn?

- PostgreSQL & PostGIS allow us to manage geospatial data in an [RDBMS](https://en.wikipedia.org/wiki/Relational_database_management_system)

--

- PgAdmin4 and QGIS allow us to connect to PostgreSQL/PostGIS to work with our data

--

- `shp2pgsql` allows us to load shapefiles into PostgreSQL tables from a command line

--

- PostGIS functions allow us to do spatial tasks in PostgreSQL

--

- GeoServer allows us to create open geospatial web services using our own source data

---

class: impact

# Homework questions?  Show & tell!

---

class: impact

# A note about today's session...

---

## Today we'll be editing code...!

--

- don't worry! I promise, it will be simple stuff

--

- this means, we'll all need to have a text editor installed on our laptops

- if you already have a preferred text editor installed, then you are free to use it

- if not, I highly recommend [Atom](https://atom.io/) - it's also free and open-source (and it's totally awesome)

--

- if anyone does not have a text editor installed already, let's take care of that now...

---

class: impact

# Part 4: Serving PostGIS data with GeoServer

---

## Adding layers from a PostGIS store in GeoServer

- as before with a shapefile, we first must create a new PostGIS store pointing to our data source:
    
    - go to "Stores" > "Add a new Store" and select "PostGIS"
    
    - set the "Data Source Name", "database" and "user" values for your connection
    
- once you've created your store, you will be able to publish your database entities (tables and views) as "layers" 

---

## PostgreSQL Views

- PostgreSQL (like any RDBMS) allows us to create [views](https://en.wikipedia.org/wiki/View_%28SQL%29)

- database views are entities that can store queries and return their results

- you can select from views just as you would from a database table

- GeoServer allows you to create layers from PostGIS views, which is a nice way to visualize the result of spatial queries

---

class: impact

## Demo: Creating a PostgreSQL view, adding to GeoServer

---

class: impact

## Hands-on Exercise

---

## Ex. 4: Adding PostGIS layers to GeoServer

On your laptop, do the following:

1. create a new view in PostgreSQL using the join query that we wrote in [Ex. 2](index.html#31)

1. start GeoServer

1. using the GeoServer web interface:

    1. create a new PostGIS store pointing to your local database
    
    1. create a layer from the view that you created above
    
    1. preview your layer in the "Layer Preview"

1. **extra credit 1**: create a new style in GeoServer to style your country polygons based on the place count value (see [this example from the cookbook](http://docs.geoserver.org/stable/en/user/styling/sld/cookbook/polygons.html#attribute-based-polygon))

1. **extra credit 2**: create a layer using another table, for example, a table created as part of your homework; restyle that layer

---

class: impact

## 10 minute break

---

class: impact

# Part 5: Leaflet basics & raster layers

---

## What is Leaflet?

[Leaflet](http://leafletjs.com/) is an open-source JavaScript library for making web maps.

--

- JavaScript libraries are bundles of code that we can use to help us do powerful things

- Leaflet provides a simple [API](https://en.wikipedia.org/wiki/Application_programming_interface) for creating web maps that use a variety of data sources ([complete Leaflet API documentation here](http://leafletjs.com/reference-1.1.0.html))

- The project website includes [lots of good tutorials](http://leafletjs.com/examples.html) on how to create different kinds of maps

--

- You can include Leaflet on a webpage by adding the following tags:

```HTML
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.1.0/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet@1.1.0/dist/leaflet.js"><\/script>
```

---

## Creating a Leaflet map

Once you've included the library, you can create a (blank) Leaflet map that fills the entire window by adding the following to your page:

```HTML
<style media="screen">
    #map {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
    }
</style>

<div id="map"></div>

<script type="text/javascript">
    var map = L.map('map').setView([0,0], 2);
<\/script>
```

---

## Leaflet & GeoServer

- To display data on a Leaflet map, you must add "layers":

    - layers can be either raster or vector
    
    - [Leaflet's API](http://leafletjs.com/reference-1.1.0.html) supports creating layers from different data sources
    
--

- GeoServer and Leaflet can easily be integrated since they both support open geospatial standards, for example:

    - GeoServer supports serving layers as tiles using the open [WMS](https://en.wikipedia.org/wiki/Web_Map_Service) standard
    
    - Leaflet's API includes [support for creating a `TileLayer` from WMS](http://leafletjs.com/reference-1.1.0.html#tilelayer-wms)

    - Given a GeoServer instance running at http://localhost:8080/geoserver with a layer called "cite:countries", a WMS layer can be added to a Leaflet map (`map`) like so:
    
```JS
L.tileLayer.wms('http://localhost:8080/geoserver/ows?', {
    layers: 'cite:countries'
}).addTo(map);
``` 

---

class: impact

## Demo: Creating a Leaflet map with a raster layer from GeoServer

---

class: impact

## Hands-on Exercise

---

## Ex. 5: Creating a Leaflet map

On your laptop, do the following:

1. create a static web page (`.html` file)
    
1. include the Leaflet library on the page

1. create a Leaflet map

1. add the layer you created in [Ex. 4](#19) to your web map as a WMS layer

1. **extra credit**: add the places points as another WMS layer on top of the countries layer 

---

class: impact

## 10 minute break

---

class: impact

# Part 6: More Leaflet & GeoJSON - vector layers and interactivity

---

class: geojson

## Vector data & GeoJSON

In web mapping, using vectors (instead of image tiles, for instance) has a few advantages:

- vectors can be styled on the client, which allows for visual interactivity (for example, highlighting features based on a user action)

- vectors can include related attribute data, eliminating the need to query the server again for these data

--

[GeoJSON](https://en.wikipedia.org/wiki/GeoJSON) is an open geospatial standard for representing vector data commonly used in web mapping.  Here's a simple example of a `FeatureCollection` with a single point:

```json
{
    "type": "FeatureCollection",
    "features": [
        {
            "type": "Feature",
            "geometry": {
                "type": "Point",
                "coordinates": [102.0, 0.5]
            },
            "properties": {
                "name": "my location"
            }
        }
    ]
}
```

---

## GeoServer & JSONP

[JSONP](https://en.wikipedia.org/wiki/JSONP) is a way to request JSON data across hosts as a workaround for the [Same-origin policy](https://en.wikipedia.org/wiki/Same-origin_policy).

--

- Since our webpage isn't running on the same port as GeoServer, we'll need to use JSONP to request vector data

- GeoServer supports JSONP, but it must be enabled in the settings XML file.

    - by default on macOS, the settings XML file can be found here: `/Applications/GeoServer.app/Contents/Java/webapps/geoserver/WEB-INF/web.xml`

    - to enable JSONP, find the `ENABLE_JSONP` setting in the XML file (by default it should be around line 40) and ensure that it is uncommented (remove the `<!--` before and the `-->` after) and set to `true`, like so:
    
```xml
<context-param>
  <param-name>ENABLE_JSONP</param-name>
  <param-value>true</param-value>
</context-param>
```
    
---

class: geojson

## Using jQuery to request data from GeoServer

We'll be using [jQuery](https://jquery.com/) (another open-source JavaScript library) to request data from GeoServer.  jQuery is a popular library for doing lots of common tasks on webpages, including creating [AJAX requests](https://en.wikipedia.org/wiki/Ajax_%28programming%29).

- to include jQuery on your webpage, add the following tag:

```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/2.2.4/jquery.min.js"><\/script>
```

--

- with jQuery included, we can request GeoJSON data from GeoServer like so:

```js
$.ajax({
    url: 'http://localhost:8080/geoserver/ows',
    data: {
        service: 'WFS',
        version: '1.0.0',
        request: 'GetFeature',
        typeName: 'cite:countries',
        maxFeatures: 1000,
        outputFormat: 'text/javascript'
    },
    dataType: 'jsonp',
    jsonpCallback: 'parseResponse',
    success: function (data) {
        console.log(data);
    }
});
```

---

class: geojson

## Adding a GeoJSON layer to Leaflet

Using the GeoJSON data returned from GeoServer, we can create a new Leaflet GeoJSON layer, like so:

```js
L.geoJson(data).addTo(map);
```

--

Since we're using vector data now, we are able to restyle the layer ([See the Leaflet documentation for styling options](http://leafletjs.com/reference-1.1.0.html#path-option)) and add interactivity using the Leaflet API.

For example, we can restyle the United States in green and enable a popup displaying the country name like so:

```js
L.geoJson(data, {
    style: function (feature) {
        var color = 'blue';
        if (feature.properties.name === 'United States of America') {
            color = 'green';
        }
        return {
            color: color
        };
    },
    onEachFeature: function (feature, layer) {
        layer.bindPopup(feature.properties.name);
    }
}).addTo(map);
```
---

class: mapbox

## Adding an external basemap

Often times, it's nice to overlay our own vectors on top of nice basemaps from an external provider.  Mapbox is one commonly use provider of map data that works well with Leaflet.  To use Mapbox with Leaflet:

- [Sign up for an API key here](https://www.mapbox.com/signup/)

- [Find your API key ("Default Public Token") here](https://www.mapbox.com/studio/account/tokens/)

- add a Mapbox layer to your Leaflet map, like so:

```js
L.tileLayer('https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token={accessToken}', {
    // this is the boilerplate attribution text for Mapbox layers:
    attribution: 'Map data &copy; <a href="http://openstreetmap.org">OpenStreetMap</a> contributors,'
    +' <a href="http://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © '
    +'<a href="http://mapbox.com">Mapbox</a>',
    
    // in this case, we are using the Mapbox map called "mapbox.streets"
    id: 'mapbox.streets',
    
    // here's where you'll put your API key:
    accessToken: 'YOUR API KEY HERE!'
}).addTo(map);
```

---

class: impact

## Demo: Adding a GeoJSON vector layer to Leaflet from GeoServer

---

class: impact

## Hands-on Exercise

---

## Ex. 6: Adding GeoJSON vectors to a Leaflet map

On your laptop, do the following:

1. create a layer in GeoServer using the "places" table that we loaded in [Ex. 2](index.html#31)

1. enable JSONP on your GeoServer installation

1. include jQuery on your webpage

1. retrieve "places" data from GeoServer as GeoJSON using jQuery & JSONP

1. add "places" to your Leaflet map as a GeoJSON layer

1. add a popup to the "places" points displaying the place's name and population on click

1. **extra credit 1**: add the countries layer as a vector layer.  Restyle the countries on place count and add a popup displaying the name and count.

1. **extra credit 2**: add Mapbox streets as a basemap beneath your vector overlays

---

class: impact

# Thanks!

