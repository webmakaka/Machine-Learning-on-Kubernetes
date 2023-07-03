# Chapter 9. Building Your Data Pipeline

<br/>

```
// $ eval $(minikube --profile ${PROFILE} docker-env)
// $ docker pull quay.io/ml-on-k8s/spark:3.2.0
```

<br/>

На этом шаге подготавливаем данные.

<br/>

Пересоздал environment c 0.

<br/>

### Подготавливаем данные в базе

```
$ kubectl create -f Chapter09/deployment-pg-flights-data.yaml -n ml-workshop
```

<br/>

```
$ kubectl create -f Chapter09/service-pg-flights-data.yaml -n ml-workshop
```

<br/>

```
$ POD_NAME=$(kubectl get pods -n ml-workshop -l app=pg-flights-data -o jsonpath="{.items[0].metadata.name}")
```

<br/>

```
$ echo ${POD_NAME}
```

<br/>

```
$ kubectl exec -it $POD_NAME -n ml-workshop -- bash
```

<br/>

```
# psql -U postgres
```

<br/>

```
postgres=# select count(1) from flights;
```

<br/>

```
  count
---------
 5819079
(1 row)
```

<br/>

### Подготавливаем данные в minio

<br/>

```
// minio / minio123
https://minio.192.168.49.2.nip.io
```

<br/>

```
Buckets -> Create Bucket > airport-data
Buckets -> Create Bucket > flights-data
```

<br/>

airport-data Load 2 files from Chapter09/data/

<br/>

- airports.csv
- airlines.csv

<br/>

### Пробуем загрузить данные в jupyterhub

<br/>

```
// mluser / mluser
https://jupyterhub.192.168.49.2.nip.io/
```

<br/>

```
Notebook Image: Elyra Notebook Image with Spark

// Large не стартовал. Там запрос на 4 CPU 16 GB
// Medium тоже.
// Small отработал после отключения Grafana и Prometheus
Container size: Small

Start server
```

<br/>

```
$ k9s -n ml-workshop
```

<br/>

```
$ kubectl get pods -n ml-workshop | grep spark
spark-cluster-admin-m-9p8dx                   1/1     Running     0             5m46s
spark-cluster-admin-w-7jqhp                   1/1     Running     0             5m46s
spark-cluster-admin-w-pw9dw                   1/1     Running     0             5m46s
spark-operator-7484d45984-5wsvt               1/1     Running     0             109m
```

<br/>

```
m - master
w - worker
```

<br/>

```
jupyter -> RUN -> Chapter09/explore_data.ipynb
```

<br/>

Последний блок работает так.

<br/>

```python
import os
from pyspark.sql import SparkSession
spark = SparkSession \
    .builder \
    .appName("Python Spark S3 example") \
    .config("spark.hadoop.fs.s3a.endpoint", "http://minio-ml-workshop:9000")\
    .config("spark.hadoop.fs.s3a.access.key", 'minio')\
    .config("spark.hadoop.fs.s3a.secret.key", 'minio123')\
    .config("spark.hadoop.fs.s3a.impl", "org.apache.hadoop.fs.s3a.S3AFileSystem")\
    .config("spark.hadoop.fs.s3a.multipart.size", "104857600")\
    .config("spark.hadoop.fs.s3a.path.style.access", "true")\
    .getOrCreate()

dfAirlines = spark.read\
                .options(delimeter=',', inferSchema='True', header='True') \
                .csv("s3a://airport-data/airlines.csv")

dfAirlines.printSchema()

dfAirports = spark.read \
                .options(delimiter=',', inferSchema='True', header='True') \
                .csv("s3a://airport-data/airports.csv")

dfAirports.printSchema()

dfAirports.show(truncate=False)
dfAirlines.show(truncate=False)

print(dfAirports.count())
print(dfAirlines.count())

spark.stop()
```

<br/>

### Designing and building the pipeline

<br/>

```
RUN -> Chapter09/merge_data.ipynb
RUN -> Chapter09/clean_data.ipynb
```

<br/>

https://spark-cluster-mluser.192.168.49.2.nip.io

<br/>

### Building and executing a data pipeline using Airflow

<br/>

https://jupyterhub.192.168.49.2.nip.io/

<br/>

Runtime Images (from left) -> Add

<br/>

```
Name: Airflow Python Runner

Description: A container with Python runtime

Source: quay.io/ml-on-k8s/airflow-python-runner:0.0.11

Image Pull Policy: IfNotPresent

SAVE & CLOSE
```

<br/>

```
Name: AirFlow PySpark Runner

Description: A container with notebook and pyspark to enable execution of PySpark code

Source: quay.io/ml-on-k8s/elyra-spark:0.0.4

Image Pull Policy: IfNotPresent

SAVE & CLOSE
```

<br/>

```
RUN -> Chapter09\flights.pipeline
```

<br/>

```
Pipeline Name: flights
Runntime Platform: Apache Airflow runtime
Runtime Configuration: MyAirflow
```

<br/>

```
// mluser / mluser
https://airflow.192.168.49.2.nip.io
```

<br/>

```
// minio / minio123
// Смотрим flights-data (вроде)
https://minio.192.168.49.2.nip.io
```
