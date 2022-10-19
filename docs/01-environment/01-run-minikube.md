# Запуск minikube

<br/>

Делаю:  
13.09.2022

<br/>

```
// 8 ядер маловато будет!
$ export \
    PROFILE=marley-minikube \
    CPUS=8 \
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

    // При необходимости можно будет удалить профиль и все созданное в профиле следующей командой
    // $ minikube --profile ${PROFILE} stop && minikube --profile ${PROFILE} delete

    // Стартовать остановленный minikube
    // $ minikube --profile ${PROFILE} start

<br/>

### Закачиваем образы

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
