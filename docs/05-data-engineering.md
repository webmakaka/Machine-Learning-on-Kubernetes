# Chapter 5. Data Engineering

<br/>

### Validating the JupyterHub installation

<br/>

```
$ kubectl get ingress -n ml-workshop
NAME                   CLASS   HOSTS                            ADDRESS        PORTS     AGE
ap-airflow2            nginx   airflow.192.168.49.2.nip.io      192.168.49.2   80, 443   3m36s
grafana                nginx   grafana.192.168.49.2.nip.io      192.168.49.2   80, 443   3m36s
jupyterhub             nginx   jupyterhub.192.168.49.2.nip.io   192.168.49.2   80, 443   3m39s
minio-ml-workshop-ui   nginx   minio.192.168.49.2.nip.io        192.168.49.2   80, 443   3m36s
mlflow                 nginx   mlflow.192.168.49.2.nip.io       192.168.49.2   80, 443   3m36s
```

<br/>

```
// mluser / mluser
https://jupyterhub.192.168.49.2.nip.io

Image: Base Elyra Notebook
Container size: Default
```

<br/>

```
// Посмотреть, что запустился еще 1 контейнер
$ kubectl get pods -n ml-workshop | grep mluser
```

<br/>

### Creating a Spark cluster

<br/>

```
$ kubectl get pods -n ml-workshop | grep spark-operator
spark-operator-6bc4f8f5f8-6x5t6                1/1     Running     0             80m
```

<br/>

```
$ kubectl create -f Chapter05/simple-spark-cluster.yaml -n ml-workshop
```

<br/>

```
$ kubectl get pods -n ml-workshop | grep simple-spark
```

<br/>

Select the Elyra Notebook Image with Spark image and the Small container size.

<br/>

```
https://spark-cluster-mluser.192.168.49.2.nip.io
```
