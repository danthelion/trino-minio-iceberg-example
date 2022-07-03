```shell
docker-compose up
```

Configure s3cmd

```shell
s3cmd --config minio.s3cfg --configure
```

Load data into minio

```shell
s3cmd --config minio.s3cfg mb s3://iris
s3cmd --config minio.s3cfg put data/iris.parq s3://iris
```

```shell
./trino-cli-388-executable.jar --server http://localhost:8080 --catalog iceberg
```

```sql
CREATE SCHEMA IF NOT EXISTS iris
    WITH (location = 's3a://iris/');

CREATE TABLE IF NOT EXISTS iris.iris_parquet
(
    sepal_length DOUBLE,
    sepal_width  DOUBLE,
    petal_length DOUBLE,
    petal_width  DOUBLE,
    class        VARCHAR
)
WITH ( location = 's3a://iris/',
    format = 'PARQUET');
```
