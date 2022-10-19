# Chapter 6. Machine Learning Engineering

<br/>

```
// minio / minio123
https://minio.192.168.49.2.nip.io
```

<br/>

```
// mluser / mluser
https://mlflow.192.168.49.2.nip.io
```

<br/>

```
// mluser / mluser
https://jupyterhub.192.168.49.2.nip.io/hub/spawn
```

<br/>

```
Image: SciKit v.1.10 - Elyra Notebook Image
Container size: Small

Variable name: AWS_SECRET_ACCESS_KEY
Variable value: minio123

Secret: no
```

<br/>

```
$ git clone http://github.com/webmakaka/Machine-Learning-on-Kubernetes.git

Run -> Chapter06/hellomlflow.ipynb

RUN -> Chapter06/hellomlflow-custom.ipynb
```

<!--

<br/>

### Демонстрация создания своего image

```
$ docker build -t scikit-notebook:v1.1.0 -f chapter6/CustomNotebookDockerfile ./chapter6/.
```

<br/>

```
$ docker tag scikit-notebook:v1.1.0 quay.io/ml-on-k8s/scikit-notebook:v1.1.0
```

<br/>

```
$ docker push quay.io/ml-on-k8s/scikit-notebook:v1.1.0
```

<br/>

```
$ vi manifests/jupyterhub-images/base/customnotebook-imagestream.yaml
```

-->
