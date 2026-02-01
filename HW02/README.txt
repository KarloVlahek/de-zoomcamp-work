# de-zoomcamp-work

## Question 1. Add the following labels to the `set_labels` task in the [scheduling workflow](pg-schedule.yaml):

```yaml
size_mb: "{{ (fileSize(render(vars.data)) / 1024 / 1024) | numberFormat('#.##') }}"
```
`128.3 MiB`

## Question 2. `green_tripdata_2020-04.csv`

## Question 3. 

```SQL
SELECT COUNT(*)
FROM `kestra-sandbox-demo-486022.zoomcamp.yellow_tripdata`
WHERE filename LIKE 'yellow_tripdata_2020-%%.csv'
```
`24,648,499`

## Question 4.

```SQL
SELECT COUNT(*)
FROM `kestra-sandbox-demo-486022.zoomcamp.green_tripdata`
WHERE filename LIKE 'green_tripdata_2020-%%.csv'
```
`1,734,051`

## Question 5.

```SQL
SELECT COUNT(*)
FROM `kestra-sandbox-demo-486022.zoomcamp.yellow_tripdata`
WHERE filename = "yellow_tripdata_2021-03.csv"
```
`1,925,152`

## Question 6. Found in the Kestra documentation.
`Add a timezone property set to America/New_York in the Schedule trigger configuration`