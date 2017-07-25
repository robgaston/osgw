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

- PgAdmin4 and QGIS allow us to connect to PostgreSQL/PostGIS to work with our data

- `shp2pgsql` allows us to load shapefiles into PostgreSQL tables from a command line

- PostGIS functions allow us to do spatial tasks in PostgreSQL

- GeoServer allows us to create open geospatial web services using our own source data

---

class: impact

# Homework show & tell!

---

class: impact

# A note about text editors...

---

## Today we'll be editing code...!

- this means, we'll all need to have a text editor installed on our laptops

- if you already have a preferred text editor installed, then you are free to use it

- if not, I highly recommend [Atom](https://atom.io/)

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

1. **extra credit 1**: create a new style in GeoServer to style your country polygons based on the place count value

1. **extra credit 2**: create a layer using another table, for example, a table created as part of your homework; restyle that layer

---

class: impact

## 10 minute break

---

class: impact

# Part 5: Leaflet basics & raster layers

---

class: impact

## Hands-on Exercise

---

class: impact

## 10 minute break

---

class: impact

# Part 6: More Leaflet & GeoJSON - vector layers and interactivity

---

class: impact

## Hands-on Exercise

---

class: impact

# Thanks!

