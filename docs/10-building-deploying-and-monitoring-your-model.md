# [UNKNOWN!] Chapter 10. Building, Deploying, and Monitoring Your Model

<br/>

На этом шаге строим модель ML.

<br/>

```
// mluser / mluser
https://jupyterhub.192.168.49.2.nip.io

Image: SciKit v.1.10 - Elyra Notebook Image
// Container size: Medium
Container size: Small

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

Register Model -> flights-ontime

<br/>

### Deploying the model as a service

<br/>

Chapter10/model_deploy_pipeline/model_build_push/Transformer.py

Chapter10/model_deploy_pipeline/flights_model.pipeline

<br/>

### Calling your model

```
$ cd Chapter10/inference
```

<br/>

```
$ curl -vvvvk --header "content-type: application/json" -X POST -d @data.json https://flights-ontime.192.168.49.2.nip.io/api/v1.0/predictions; done
```

<br/>

### Monitoring your model

```
// mluser / mluser
https://grafna.192.168.49.2.nip.io
```

<br/>

Configuration -> Data sources -> Prometheus

<br/>

```
Name: Prometheus
URL: http://prometheus-operated:9090
```

<br/>

```
Import -> Chapter10/grafana-dashboard/sample-seldon-dashboard.json
```

<br/>

```
Name: Flights Prediction Analytics
```
