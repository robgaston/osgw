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
    left join places 
    on st_contains(countries.geom, places.geom) 
    group by countries.gid;
```

Ex. 3
http://docs.geoserver.org/stable/en/user/styling/sld/cookbook/polygons.html#attribute-based-polygon

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
