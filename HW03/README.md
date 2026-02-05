#de-zoomcamp-work

## Pre-Question Set-Up
### External Table
```SQL
CREATE OR REPLACE EXTERNAL TABLE `kestra-sandbox-demo-486022.hw03.yellow_taxi_tripdata_012024_062024`
OPTIONS (
  format = 'parquet',
  uris = ['gs://dezoomcamp_hw3_2026-123456789/yellow_tripdata_2024-*.parquet']
);
```
### Materialized Table with file name
```SQL
CREATE OR REPLACE TABLE `kestra-sandbox-demo-486022.hw03.yellow_taxi_tripdata_012024_062024_materialized` AS
SELECT *, _FILE_NAME as src_file
FROM `kestra-sandbox-demo-486022.hw03.yellow_taxi_tripdata_012024_062024`;
```

## Question 1. Counting records
### Answer 1.

```SQL
SELECT COUNT(*)
FROM `kestra-sandbox-demo-486022.hw03.yellow_taxi_tripdata_012024_062024`
```
`20332093`

## Question 2. Data read estimation
### Answer 2.
```SQL
SELECT COUNT(DISTINCT(PULocationID))
FROM `kestra-sandbox-demo-486022.hw03.yellow_taxi_tripdata_012024_062024`; --This query will process 0 B when run.

SELECT COUNT(DISTINCT(PULocationID))
FROM `kestra-sandbox-demo-486022.hw03.yellow_taxi_tripdata_012024_062024_materialized`; --This query will process 155.12 MB when run.
```
`0 MB for the External Table and 155.12 MB for the Materialized Table`

## Question 3.  Understanding columnar storage
### Answer 3.
```SQL
FROM `kestra-sandbox-demo-486022.hw03.yellow_taxi_tripdata_012024_062024_materialized`; --This query will process 155.12 MB when run.

SELECT PULocationID, DOLocationID
FROM `kestra-sandbox-demo-486022.hw03.yellow_taxi_tripdata_012024_062024_materialized`; --This query will process 310.24 MB when run.
```
`BigQuery is a columnar database, and it only scans the specific columns requested in the query. Querying two columns (PULocationID, DOLocationID) requires reading more data than querying one column (PULocationID), leading to a higher estimated number of bytes processed.`

## Question 4. Counting zero fare trips
### Answer 4.
```SQL
SELECT COUNT(*)
FROM `kestra-sandbox-demo-486022.hw03.yellow_taxi_tripdata_012024_062024`
WHERE fare_amount = 0
```
`8333`

## Question 5. Partitioning and clustering
### Answer 5.
```SQL
CREATE OR REPLACE TABLE `kestra-sandbox-demo-486022.hw03.yellow_taxi_tripdata_partitioned_and_clustered`
PARTITION BY DATE(tpep_dropoff_datetime)
CLUSTER BY VendorID AS
SELECT *
FROM `kestra-sandbox-demo-486022.hw03.yellow_taxi_tripdata_012024_062024`;
```
`Partition by tpep_dropoff_datetime and Cluster on VendorID`

## Question 6. Partition benefits
### Answer 6.
```SQL
SELECT DISTINCT(VendorID)
FROM `kestra-sandbox-demo-486022.hw03.yellow_taxi_tripdata_012024_062024_materialized`
WHERE tpep_dropoff_datetime >= '2024-03-01' AND tpep_dropoff_datetime <= '2024-03-15' --This query will process 310.24 MB when run.

SELECT DISTINCT(VendorID)
FROM `kestra-sandbox-demo-486022.hw03.yellow_taxi_tripdata_partitioned_and_clustered`
WHERE tpep_dropoff_datetime >= '2024-03-01' AND tpep_dropoff_datetime <= '2024-03-15' --This query will process 26.84 when run.
```
`310.24 MB for non-partitioned table and 26.84 MB for the partitioned table`

## Question 7. External table storage
### Answer 7. `GCP Bucket`

## Question 8. Clustering best practices
### Answer 8. `False`

## Question 9. Understanding table scans
### Answer 9. `It estimates 0 bytes will be read. If the query touches no columns (e.g., COUNT(*)), BQ counts 0 columns scanned and reports 0 MB.`