# Run minikube

<br/>

Date:  
19.01.2023 - not working anymore

<br/>

```
$ lsb_release -a
No LSB modules are available.
Distributor ID:	Linuxmint
Description:	Linux Mint 20.3
Release:	20.3
Codename:	una
```

<br/>

```
$ grep 'cpu cores' /proc/cpuinfo | uniq
cpu cores	: 12

$ echo "CPU threads: $(grep -c processor /proc/cpuinfo)"
CPU threads: 24
```

<br/>

```
$ free -h
              total        used        free      shared  buff/cache   available
Mem:           31Gi       3,3Gi        20Gi       306Mi       7,8Gi        27Gi
Swap:         2,0Gi          0B       2,0Gi
```

<br/>

```
$ kubectl version --client --short
Flag --short has been deprecated, and will be removed in the future. The --short output will become the default.
Client Version: v1.25.0
Kustomize Version: v4.5.7
```

<br/>

```
$ minikube version
minikube version: v1.28.0
```

<br/>

```
$ docker --version
Docker version 20.10.22, build 3a2c30b
```

<br/>

```
$ kustomize version
{Version:kustomize/v4.5.7 GitCommit:56d82a8378dfc8dc3b3b1085e5a6e67b82966bd7 BuildDate:2022-08-02T16:35:54Z GoOs:linux GoArch:amd64}
```

<br/>

```
$ export \
    PROFILE=marley-minikube \
    CPUS=10 \
    MEMORY=30G \
    HDD=80G \
    DRIVER=docker \
    KUBERNETES_VERSION=v1.24.4
```

<br/>

```
$ {
    minikube --profile ${PROFILE} config set memory ${MEMORY}
    minikube --profile ${PROFILE} config set cpus ${CPUS}
    minikube --profile ${PROFILE} config set disk-size ${HDD}

    minikube --profile ${PROFILE} config set driver ${DRIVER}

    minikube --profile ${PROFILE} config set kubernetes-version ${KUBERNETES_VERSION}
    minikube start --profile ${PROFILE} --embed-certs

    // Enable ingress
    minikube addons --profile ${PROFILE} enable ingress

    // Enable registry
    // minikube addons --profile ${PROFILE} enable registry
}
```

<br/>

```
Creating docker container (CPUs=10, Memory=30720MB) ...
```

<br/>

    // При необходимости можно будет удалить профиль и все созданное в профиле следующей командой
    // $ minikube --profile ${PROFILE} stop && minikube --profile ${PROFILE} delete

    // Стартовать остановленный minikube
    // $ minikube --profile ${PROFILE} start

<br/>

### Download all needed images at once

<br/>

```
$ eval $(minikube docker-env)
```

<br/>

```
$ {
    docker pull quay.io/ml-on-k8s/redis-6:1-25
    docker pull quay.io/ml-aml-workshop/postgresql-96
    docker pull quay.io/ml-aml-workshop/mlflow:0.0.2

    docker pull quay.io/ml-on-k8s/airflow:2.2.3.scheduler
    docker pull quay.io/ml-on-k8s/airflow:2.2.3.web.keycloak
    docker pull quay.io/ml-on-k8s/airflow:2.2.3.worker

    docker pull quay.io/ml-on-k8s/hellomlflow-manual:1.0.0

    docker pull quay.io/ml-on-k8s/scikit-notebook:v1.2.0
    docker pull quay.io/ml-on-k8s/elyra-spark:0.0.4

    docker pull quay.io/ml-on-k8s/spark:3.2.0
    docker pull quay.io/ml-on-k8s/spark-operator:1.3.4

    docker pull quay.io/ml-on-k8s/flights-data:5.0

    docker pull quay.io/ml-on-k8s/container-model:2.0.0

    docker pull quay.io/ml-on-k8s/kaniko-container-builder:1.0.0
    docker pull quay.io/ml-on-k8s/airflow-python-runner:0.0.11
}
```

<!--
```
$ minikube --profile ${PROFILE} kubectl -- get sc
NAME                 PROVISIONER                RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
standard (default)   k8s.io/minikube-hostpath   Delete          Immediate           false                  3h4m
``` -->
