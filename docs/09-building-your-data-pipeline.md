# Chapter 9. Building Your Data Pipeline

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
$ watch kubectl get pods -n ml-workshop
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

Загрузить 2 файла из Chapter09/data/

<br/>

- airlines.csv
- airports.csv

<br/>

### Пробуем загрузить данные в jupyterhub

<br/>

```
// mluser / mluser
https://jupyterhub.192.168.49.2.nip.io/hub/spawn
```

<br/>

```
Elyra Notebook Image with Spark

// Large не стартовал. Там запрос на 4 CPU 16 GB
// Medium не помню. Small отработал после отключения Grafana и Prometheus
Container size: Small

Start server
```

<br/>

Clone: https://github.com/webmakaka/Machine-Learning-on-Kubernetes.git

<br/>

```
RUN -> Chapter09/explore_data.ipynb
```

<br/>

Последний блок завершился ошибкой. М.б. из-за того, что не Large

<br/>

### Designing and building the pipeline

<!--
Replicationcontrollers
-->

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

Runtime Images (слева) -> Добавить

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
