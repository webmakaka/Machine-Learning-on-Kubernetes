# [FAILED!] Chapter 10. Building, Deploying, and Monitoring Your Model

<br/>

На этом шаге строим модель ML.

<br/>

```
// mluser / mluser
https://jupyterhub.192.168.49.2.nip.io

Image: SciKit v.1.10 - Elyra Notebook Image
Container size: Medium

Variable name: AWS_SECRET_ACCESS_KEY
Variable value: minio123

Secret: no
```

<br/>

```
RUN -> Chapter10/visualize.ipynb
```

<br/>

### Building and tuning your model using JupyterHub

<br/>

```
RUN -> Chapter10/experiments.ipynb
```

<br/>

### Tracking model experiments

<br/>

```
// mluser / mluser
https://mlflow.192.168.49.2.nip.io
```

<br/>

FlightsDelay-mluser

<br/>

<br/>

```
MLFlow > Experiment > Artifacts > Model > Register Model > flights-ontime

MLFlow > Models > flights-ontime > Stage > Production
```

<br/>

### Deploying the model as a service

<br/>

Chapter10/model_deploy_pipeline/flights_model.pipeline

<br/>

**build_push_image.py**

Runtime Image: Kaniko Container Builder

<br/>

```
Environment Variables

MODEL_NAME=flights-ontime
MODEL_VERSION=1
CONTAINER_REGISTRY=https://index.docker.io/v1/
CONTAINER_REGISTRY_USER=webmakaka
CONTAINER_REGISTRY_PASSWORD=mypassword
CONTAINER_DETAILS=webmakaka/flights-ontime:latest
```

<br/>

**deploy_model.py**

Runtime Image: Airflow Python Runner

<br/>

```
File Dependencies:

Ingress.yaml
SeldonDeploy.yaml
```

<br/>

```
Environment Variables

MODEL_NAME=flights-ontime
MODEL_VERSION=1
CONTAINER_DETAILS=webmakaka/flights-ontime:latest
CLUSTER_DOMAIN_NAME=192.168.49.2.nip.io
```

<br/>

RUN

<br/>

https://hub.docker.com/u/webmakaka/

<br/>

logs are in minio

<br/>

### Calling your model

```
$ cd Chapter10/inference
```

```
$ kubectl get ingress -n ml-workshop
```

<br/>

```
$ curl -vvvk --header "content-type: application/json" -X POST -d @data.json https://flights-ontime.192.168.49.2.nip.io/api/v1.0/predictions | jq
```

<br/>

**response**

```
{
  "data": {
    "names": [
      "t:0",
      "t:1"
    ],
    "ndarray": [
      [
        0.7724004126048876,
        0.22759958739511232
      ]
    ]
  },
  "meta": {
    "requestPath": {
      "predictor": "webmakaka/flights-ontime:latest",
      "transformer": "webmakaka/flights-ontime:latest"
    }
  }
}

```

<br/>

### Monitoring your model

```
// admin / admin
https://grafna.192.168.49.2.nip.io
```

<br/>

Configuration -> Data sources -> Prometheus

<br/>

```
Name: HPonbHsnk
URL: http://prometheus-operated:9090
```

<br/>

```
$ kubectl get service -n ml-workshop | grep prometheus-operated
```

<br/>

```
+ -> Import -> Chapter10/grafana-dashboard/sample-seldon-dashboard.json
```

<br/>

Grafana UI do not show anything.
