Ex. 2

load shp

```sh
shp2pgsql -I -s 4326 ~/Desktop/ne_50m_admin_1_states_provinces_lakes/ne_50m_admin_1_states_provinces_lakes.shp public.states | psql -U postgres -d geoserver
```

```sql
select admin, count(*), st_astext(st_union(geom))
	from public.states
	group by admin;
```

SQL queries

```sql
select admin, name, geom
	from public.states;
	
select admin, name, st_astext(geom) as wkt
	from public.states;

select distinct admin
	from public.states;

select admin, count(*), st_astext(st_union(geom)) as wkt
	from public.states
    group by admin;
```

solution

```sql
select countries.gid, count(places.geom) as place_count 
    from countries
    left join places on st_contains(countries.geom, places.geom)
    group by countries.gid;
```

Ex. 3

```xml
<Rule>
  <Name>USA</Name>
  <Title>USA</Title>
  <ogc:Filter>
	<ogc:PropertyIsEqualTo>
	  <ogc:PropertyName>admin</ogc:PropertyName>
	  <ogc:Literal>United States of America</ogc:Literal>
	</ogc:PropertyIsEqualTo>
  </ogc:Filter>
  <PolygonSymbolizer>
	<Fill>
	  <CssParameter name="fill">#66FF66</CssParameter>
	</Fill>
  </PolygonSymbolizer>
</Rule>
```

Ex.4

```sql
create view public.countries as
	select admin, count(*), st_union(geom)
		from public.states
		group by admin;
```

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<StyledLayerDescriptor version="1.0.0"
  xsi:schemaLocation="http://www.opengis.net/sld http://schemas.opengis.net/sld/1.0.0/StyledLayerDescriptor.xsd"
  xmlns="http://www.opengis.net/sld" xmlns:ogc="http://www.opengis.net/ogc"
  xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

  <NamedLayer>
    <Name>place_count</Name>
    <UserStyle>
      <Title>place count</Title>
      <FeatureTypeStyle>
        <Rule>
          <Name>less than 10</Name>
          <Title>less than 10</Title>
          <ogc:Filter>
            <ogc:PropertyIsLessThan>
              <ogc:PropertyName>count</ogc:PropertyName>
              <ogc:Literal>11</ogc:Literal>
            </ogc:PropertyIsLessThan>
          </ogc:Filter>
          <PolygonSymbolizer>
            <Fill>
              <CssParameter name="fill">#66a8ff</CssParameter>
            </Fill>
          </PolygonSymbolizer>
        </Rule>
        <Rule>
          <Name>11 - 20</Name>
          <Title>11 - 20</Title>
          <ogc:Filter>
            <ogc:And>
              <ogc:PropertyIsGreaterThan>
                <ogc:PropertyName>count</ogc:PropertyName>
                <ogc:Literal>10</ogc:Literal>
              </ogc:PropertyIsGreaterThan>
              <ogc:PropertyIsLessThan>
                <ogc:PropertyName>count</ogc:PropertyName>
                <ogc:Literal>21</ogc:Literal>
              </ogc:PropertyIsLessThan>
            </ogc:And>
          </ogc:Filter>
          <PolygonSymbolizer>
            <Fill>
              <CssParameter name="fill">#33CC33</CssParameter>
            </Fill>
          </PolygonSymbolizer>
        </Rule>
        <Rule>
          <Name>greater than 20</Name>
          <Title>greater than 20</Title>
          <ogc:Filter>
            <ogc:PropertyIsGreaterThan>
              <ogc:PropertyName>count</ogc:PropertyName>
              <ogc:Literal>20</ogc:Literal>
            </ogc:PropertyIsGreaterThan>
          </ogc:Filter>
          <PolygonSymbolizer>
            <Fill>
              <CssParameter name="fill">#f68431</CssParameter>
            </Fill>
          </PolygonSymbolizer>
        </Rule>
      </FeatureTypeStyle>
    </UserStyle>
  </NamedLayer>
</StyledLayerDescriptor>
```

Ex.5

```html
<title>OSGW</title>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.1.0/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet@1.1.0/dist/leaflet.js"></script>

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

    L.tileLayer.wms('http://localhost:8080/geoserver/ows?', {
        layers: 'cite:countries'
    }).addTo(map);
</script>
```

Ex.6

web.xml path:
- `/Applications/GeoServer.app/Contents/Java/webapps/geoserver/WEB-INF/web.xml`

```xml
<context-param>
  <param-name>ENABLE_JSONP</param-name>
  <param-value>true</param-value>
</context-param>
```

```html
<title>OSGW</title>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.1.0/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet@1.1.0/dist/leaflet.js"></script>

<script src="https://ajax.googleapis.com/ajax/libs/jquery/2.2.4/jquery.min.js"></script>

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

    L.tileLayer('https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token={accessToken}', {
		attribution: 'Map data &copy; <a href="http://openstreetmap.org">OpenStreetMap</a> contributors, <a href="http://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="http://mapbox.com">Mapbox</a>',
		id: 'mapbox.streets',
		accessToken: 'pk.eyJ1Ijoicmdhc3RvbiIsImEiOiJJYTdoRWNJIn0.MN6DrT07IEKXadCU8xpUMg'
	}).addTo(map);

	$.ajax({
        url: 'http://localhost:8080/geoserver/ows',
        data: {
            service: 'WFS',
            version: '1.0.0',
            request: 'GetFeature',
            typeName: 'cite:states',
            maxFeatures: 1000,
            outputFormat: 'text/javascript'
        },
        dataType: 'jsonp',
        jsonpCallback: 'parseResponse',
        success: function (data) {
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
        }
    });
</script>
```

