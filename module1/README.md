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

<img width="1512" alt="Longest Trip" src="https://github.com/user-attachments/assets/8f6a236b-f95d-46c9-a5b2-9872e25d6671" />


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

<img width="1512" alt="Biggest pickup zones" src="https://github.com/user-attachments/assets/3b0d628b-8f3f-44b6-bb8d-6545d3eba6f8" />


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

<img width="1512" alt="Largest tip" src="https://github.com/user-attachments/assets/3714b147-d8a7-4287-8336-3179242e6475" />
