Ex. 2

load shp

```sh
shp2pgsql -I -s 4326 ~/Downloads/ne_50m_admin_1_states_provinces_lakes/ne_50m_admin_1_states_provinces_lakes.shp public.states | psql -U postgres -d geoserver
```

SQL queries

```SQL
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

```SQL
select countries.gid, count(places.geom) as place_count 
    from countries
    join places 
    on st_contains(countries.geom, places.geom) 
    group by countries.gid;
```