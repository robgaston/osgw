# Day 1, July 12

- Part 1: PostgreSQL & PostGIS basics
	- Lecture/Walkthrough (20 min)
	- Hands-on (30 min)
		- Install PostgreSQL and PostGIS with command line tools
		- Install PgAdmin4
		- Create DB and add PostGIS extension
		- Confirm PostGIS install
- Break (5-10 min)
- Part 2: Loading and querying Geospatial data into PostGIS 
	- Lecture/Walkthrough (30 min)
	- Hands-on (40 min)
		- Download Natural Earth Shapefiles (countries & places)
		- Load shapefiles into PostgreSQL using shp2pgsql command
		- Query data with pgadmin4
		- Stretch goal: join points to countries w/ a spatial query to count places in countries
- Break (5-10 min)
- Part 3: Intro to Geoserver
	- Lecture/Walkthrough (20 min)
	- Hands-on (20 min)
		- Install Geoserver
		- Start server and confirm installation by viewing web interface
		
# Day 2, July 30

- Part 4: Serving PostGIS data with Geoserver
	- Lecture/Walkthrough (20 min)
	- Hands-on (30 min)
		- Connect PostGIS as a data source
		- Create countries and places layers
		- View OpenLayers demos for each layers
		- Stretch goal: adjust styles for country layer
- Break (5-10 min)
- Part 5: Leaflet basics & raster layers
	- Lecture/Walkthrough (20 min)
	- Hands-on (30 min)
		- Create static web page with an empty Leaflet map
		- Add countries layer from Geoserver as WMS layer
- Break (5-10 min)
- Part 6: More Leaflet & GeoJSON - vector layers and interactivity
	- Lecture/Walkthrough (30 min)
	- Hands-on (30 min)
		- Enable CORS on Geoserver installation
		- Retrieve places data from Geoserver as geojson
		- add places to Leaflet map as a vector layer
		- restyle places points using Leaflet
		- add a popup to places points displaying name and population on click
