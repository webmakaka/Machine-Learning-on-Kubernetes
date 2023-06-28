# Chapter 7. Model Deployment and Automation

<br/>

**In MLFlow mlflowdemo model should be registered**

<br/>

```
$ kubectl get pods -n ml-workshop | grep seldon
seldon-controller-manager-c5f59c56d-dxr9h              1/1     Running     0             66m
```

<br/>

### Packaging, running, and monitoring a model using Seldon Core

<br/>

```
$ kubectl get pods -n ml-workshop | grep -iE 'mlflow|minio'
```

<br/>

```
minio-ml-workshop-7fcc5dfd8-dqc65                      1/1     Running     0             67m
minio-ml-workshop-dfxc6                                0/1     Completed   2             67m
mlflow-5fb4cb5d5d-qhwgq                                2/2     Running     0             67m
mlflow-db-0                                            1/1     Running     1 (65m ago)   67m
```

<br/>

```
$ kubectl get ingresses.networking.k8s.io -n ml-workshop
```

<br/>

```
NAME                   CLASS   HOSTS                            ADDRESS        PORTS     AGE
ap-airflow2            nginx   airflow.192.168.49.2.nip.io      192.168.49.2   80, 443   67m
grafana                nginx   grafana.192.168.49.2.nip.io      192.168.49.2   80, 443   67m
jupyterhub             nginx   jupyterhub.192.168.49.2.nip.io   192.168.49.2   80, 443   67m
minio-ml-workshop-ui   nginx   minio.192.168.49.2.nip.io        192.168.49.2   80, 443   67m
mlflow                 nginx   mlflow.192.168.49.2.nip.io       192.168.49.2   80, 443   67m

```

<br/>

MLFLOW -> HelloMlFlow

Chapter07/model_deploy_pipeline/model_build_push

<br/>

Докачать файлы:

<br/>

```
model.pkl
requirements.txt
```

<br/>

Copy:

Machine-Learning-on-Kubernetes/Chapter07/model_deploy_pipeline/model_build_push

<br/>

```
$ docker build -t hellomlflow-manual:1.0.0 .

// $ docker tag hellomlflow-manual:1.0.0 <DOCKER_REGISTRY>/username/hellomlflow-manual:1.0.0

$ docker tag hellomlflow-manual:1.0.0 quay.io/marley/hellomlflow-manual:1.0.0

$ docker login quay.io

// $ docker push <DOCKER_REGISTRY>/username/hellomlflow-manual:1.0.0

$ docker push quay.io/marley/hellomlflow-manual:1.0.0
```

<br/>

```
$ vi Chapter07/manual_model_deployment/SeldonDeploy.yaml
```

<br/>

replace an image on image above

<br/>

```
- image: quay.io/marley/hellomlflow-manual:1.0.0
```

<br/>

```
$ kubectl create -f Chapter07/manual_model_deployment/SeldonDeploy.yaml -n ml-workshop
```

<br/>

```
$ kubectl get pod -n ml-workshop | grep model-test-predictor
```

<br/>

```
$ export POD_NAME=$(kubectl get pod -o=custom-columns=NAME:.metadata.name -n ml-workshop | grep model-test-predictor)
```

<br/>

```
$ echo ${POD_NAME}
```

<br/>

```
$ kubectl get pods $POD_NAME -o jsonpath='{.spec.containers[*].name}' -n ml-workshop
```

<br/>

response:

```
model-test-predictor seldon-container-engine
```

<br/>

```
$ kubectl logs -f $POD_NAME -n ml-workshop -c model-test-predictor
```

<br/>

```
$ kubectl get all -l seldon-deployment-id=model-test -n ml-workshop
```

<br/>

```
$ minikube ip --profile ${PROFILE}
```

<br/>

```
$ export MINIKUBE_IP_ADDR=192.168.49.2
```

<br/>

```
// Check
// $ envsubst < Chapter07/manual_model_deployment/Ingress.yaml
```

<br/>

```
$ envsubst < Chapter07/manual_model_deployment/Ingress.yaml | kubectl create -f - -n ml-workshop
```

<br/>

```
$ kubectl create -f Chapter07/manual_model_deployment/Ingress.yaml -n ml-workshop
```

<br/>

```
$ kubectl get ingress -n ml-workshop | grep model-test
```

<br/>

```
$ kubectl create -f Chapter07/manual_model_deployment/http-echo-service.yaml -n ml-workshop
```

<br/>

```
$ kubectl get pods -n ml-workshop | grep logger
```

<br/>

```
$ curl -vvvv -X POST 'http://model-test.192.168.49.2.nip.io/api/v1.0/predictions' --header 'Content-Type: application/json' --data-raw '{ "data": {"ndarray": [[2,1]] }}'
```

<br/>

```
$ export LOGGER_POD_NAME=$(kubectl get pod -o=custom-columns=NAME:.metadata.name -n ml-workshop | grep logger)

$ kubectl logs -f ${LOGGER_POD_NAME} -n ml-workshop
```

<br/>

response:

```
{
    "path": "/",
    "headers": {
        "host": "logger",
        "user-agent": "Go-http-client/1.1",
        "content-length": "32",
        "ce-endpoint": "predictor",
        "ce-id": "6a572f54-658c-413f-a272-dd757903bd89",
        "ce-inferenceservicename": "model-test",
        "ce-modelid": "model-test-predictor",
        "ce-namespace": "ml-workshop",
        "ce-requestid": "12b0d447-288e-42a0-a7b4-b630c8b4895c",
        "ce-source": "http://:8000/",
        "ce-specversion": "1.0",
        "ce-time": "2022-08-16T22:38:13.447502807Z",
        "ce-traceparent": "00-64564c191d5294dc3c5e578fe70b1bd8-cca0ca20e93e2036-00",
        "ce-type": "io.seldon.serving.inference.request",
        "content-type": "application/json",
        "traceparent": "00-64564c191d5294dc3c5e578fe70b1bd8-879cb100cfd08522-00",
        "accept-encoding": "gzip"
    },
    "method": "POST",
    "body": "{ \"data\": {\"ndarray\": [[2,1]] }}",
    "fresh": false,
    "hostname": "logger",
    "ip": "::ffff:172.17.0.1",
    "ips": [],
    "protocol": "http",
    "query": {},
    "subdomains": [],
    "xhr": false,
    "os": {
        "hostname": "logger-8f78f5cb6-gf785"
    },
    "connection": {},
    "json": {
        "data": {
            "ndarray": [
                [
                    2,
                    1
                ]
            ]
        }
    }
}
::ffff:172.17.0.1 - - [16/Aug/2022:22:38:13 +0000] "POST / HTTP/1.1" 200 1201 "-" "Go-http-client/1.1"
-----------------
{
    "path": "/",
    "headers": {
        "host": "logger",
        "user-agent": "Go-http-client/1.1",
        "content-length": "169",
        "ce-endpoint": "predictor",
        "ce-id": "5db25f80-fb6e-4785-b93f-9e3349bf1883",
        "ce-inferenceservicename": "model-test",
        "ce-modelid": "model-test-predictor",
        "ce-namespace": "ml-workshop",
        "ce-requestid": "12b0d447-288e-42a0-a7b4-b630c8b4895c",
        "ce-source": "http://:8000/",
        "ce-specversion": "1.0",
        "ce-time": "2022-08-16T22:38:13.453580537Z",
        "ce-traceparent": "00-8d74a48068405937eb9fee5be44e4477-429898e0b462eb0e-00",
        "ce-type": "io.seldon.serving.inference.response",
        "content-type": "application/json",
        "traceparent": "00-8d74a48068405937eb9fee5be44e4477-fd937fc09af450fb-00",
        "accept-encoding": "gzip"
    },
    "method": "POST",
    "body": "{\"data\":{\"names\":[\"t:0\",\"t:1\",\"t:2\",\"t:3\"],\"ndarray\":[[0.25,0.25,0.25,0.25]]},\"meta\":{\"requestPath\":{\"model-test-predictor\":\"quay.io/marley/hellomlflow-manual:1.0.0\"}}}\n",
    "fresh": false,
    "hostname": "logger",
    "ip": "::ffff:172.17.0.1",
    "ips": [],
    "protocol": "http",
    "query": {},
    "subdomains": [],
    "xhr": false,
    "os": {
        "hostname": "logger-8f78f5cb6-gf785"
    },
    "connection": {},
    "json": {
        "data": {
            "names": [
                "t:0",
                "t:1",
                "t:2",
                "t:3"
            ],
            "ndarray": [
                [
                    0.25,
                    0.25,
                    0.25,
                    0.25
                ]
            ]
        },
        "meta": {
            "requestPath": {
                "model-test-predictor": "quay.io/marley/hellomlflow-manual:1.0.0"
            }
        }
    }
}

```

<br/>

http://model-test.192.168.49.2.nip.io/prometheus

<br/>

### Apache Airflow

// Create an empty repo  
https://github.com/wildmakaka/airflow-dags

<br/>

```
$ kubectl get pods -n ml-workshop | grep airflow
app-aflow-airflow-scheduler-6f9c44866d-hx9vw                   2/2     Running     2 (124m ago)    23h
app-aflow-airflow-web-6d9587698-4hk9w                          2/2     Running     4 (130m ago)    23h
app-aflow-airflow-worker-0                                     2/2     Running     2 (5h51m ago)   23h

```

<br/>

```
$ kubectl get ingress -n ml-workshop | grep airflow
```

<br/>

```
https://airflow.192.168.49.2.nip.io
```

<br/>

### Configuring Airflow runtime images

<br/>

https://jupyterhub.192.168.49.2.nip.io/

<br/>

```
Image: SciKit v.1.10 - Elyra Notebook Image
Container size: Small

Variable name: AWS_SECRET_ACCESS_KEY
Variable value: minio123

Secret: no
```

<br/>

Machine-Learning-on-Kubernetes/Chapter07/model_deploy_pipeline/

<br/>

File -> New -> Pipeline Editor

Rename hello_world.pipeline

<br/>

File > New Python File

Создать 2 файла

hello.py and world.py

<br/>

с содержимым:

```python
print('Hello airflow!')
```

Соединить стрелочкой.

<br/>

Runtime Images (слева) -> Добавить

<br/>

```
Name: Kaniko Container Builder

Description: A Kaniko container for building

Source: quay.io/ml-on-k8s/kaniko-container-builder:1.0.0

Image Pull Policy: IfNotPresent

SAVE & CLOSE
```

<br/>

```
Name: Airflow Python Runner

Description: A container with Python runtime

Source: quay.io/ml-on-k8s/airflow-python-runner:0.0.11

Image Pull Policy: IfNotPresent

SAVE & CLOSE
```

<br/>

Переоткрыть pipeline

```
hello.py -> Kaniko Container Builder
world.py -> Airflow Python Runner
```

<br/>

Runtimes (слева) + New Apache Airflow runtime

<br/>

```
Name: MyAirflow

Apache Airflow UI Endpoint: https://airflow.192.168.49.2.nip.io
Apache Airflow User Namespace: ml-workshop
Github API Endpoint: https://api.github.com
GitHub DAG Repository: wildmakaka/airflow-dags
GitHub DAG Repository Branch: main
Github Personal Access Token: YOUR_GITHUB_TOKEN


Cloud Object Storage Endpoint: http://minio-ml-workshop:9000
Cloud Object Storage Username: minio
Cloud Object Storage Password: minio123
Cloud Object Storage Bucket Name: airflow
```

<br/>

```
Play

Pipeline Name: hello_world
Runtime Platform: Apache Airflow runtime
Runtime Configuration: MyAirflow
```

<br/>

В бранче появился python скрипт.

https://github.com/wildmakaka/airflow-dags/tree/main

<br/>

https://airflow.192.168.49.2.nip.io/home

Появился DAG

<br/>

### Automating ML model deployments in Airflow

<br/>

model_deploy.pipeline

<br/>

Перетаскиваем

model_build_push -> build_push_image.py

model_deploy -> deploy_model.py

<br/>

**build_push_image.py**

Kaniko Container Builder

<br/>

File Dependencies

```
Dockerfile
base_requirements.txt
Predictor.py
```

<br/>

```
Environment Variables

MODEL_NAME=mlflowdemo
MODEL_VERSION=1
CONTAINER_REGISTRY=https://index.docker.io/v1/
CONTAINER_REGISTRY_USER=webmakaka
CONTAINER_REGISTRY_PASSWORD=mypassword
CONTAINER_DETAILS=webmakaka/mlflowdemo:latest
```

<br/>

**deploy_model.py**

Airflow Python Runner

<br/>

```
File Dependencies:

Ingress.yaml
SeldonDeploy.yaml
```

<br/>

```
Environment Variables

MODEL_NAME=mlflowdemo
MODEL_VERSION=1
CONTAINER_DETAILS=webmakaka/mlflowdemo:latest
CLUSTER_DOMAIN_NAME=192.168.49.2.nip.io
```

<br/>

```
Play

Pipeline Name: model_deploy
Runtime Platform: Apache Airflow runtime
Runtime Configuration: MyAirflow
```
