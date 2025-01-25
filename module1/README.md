### Trip Segmentation Count

### Longest Trip
```
SELECT 
    DATE(lpep_pickup_datetime) AS day, 
    trip_distance
FROM 
    "green_taxi_data" AS g
ORDER BY 
    trip_distance DESC;
```

### Biggest pickup zones
```
SELECT
    t."Zone",
    SUM(g.total_amount) AS total_amount
FROM 
    taxi_zone_lookup AS t
JOIN 
    "green_taxi_data" AS g
ON 
    t."LocationID" = g."PULocationID"
WHERE 
    DATE(g.lpep_pickup_datetime) = '2019-10-18'
GROUP BY 
    t."Zone"
HAVING 
    SUM(g.total_amount) > 13000;
```

### Largest tip
```
SELECT joined_table.pickup_zone, joined_table.tip_amount, filtered_lookup.dropoff_zone
FROM (
  SELECT t."Zone" AS Pickup_Zone, g.lpep_pickup_datetime, g.lpep_dropoff_datetime, g."DOLocationID", t."LocationID", g.tip_amount
  FROM "green_taxi_data" g, taxi_zone_lookup t
  WHERE g."PULocationID" = t."LocationID" AND t."Zone" = 'East Harlem North'
) joined_table,
(
  SELECT "Zone" AS Dropoff_Zone, "LocationID"
  FROM taxi_zone_lookup
) filtered_lookup
WHERE filtered_lookup."LocationID" = joined_table."DOLocationID"
ORDER BY joined_table.tip_amount DESC;
```