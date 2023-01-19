# Open Data Hub (ODH) operator installation

<br/>

```
$ cd ~/tmp/
$ git clone https://github.com/webmakaka/Machine-Learning-on-Kubernetes.git
$ cd ~/tmp/Machine-Learning-on-Kubernetes/
```

<br/>

```
$ kubectl create -f Chapter04/catalog-source.yaml
$ kubectl create -f Chapter04/odh-subscription.yaml
```

<br/>

```
// 120 sec
$ watch kubectl get pods -n operators
NAME                                   READY   STATUS    RESTARTS   AGE
opendatahub-operator-b5f4c5757-d9td2   1/1     Running   0          15s
```

<br/>

```
$ kubectl get packagemanifests -o wide -n olm | grep -I opendatahub
```

<br/>

**response:**

```
opendatahub-operator                        Community Operators Red Hat   52s
```
