# Open Data Hub (ODH) operator installation

https://github.com/opendatahub-io/opendatahub-operator

https://dev.operatorhub.io/operator/opendatahub-operator

<!-- <br/>

```
$ kubectl create -f https://dev.operatorhub.io/install/opendatahub-operator.yaml
$ watch kubectl get csv -n operators
``` -->

<br/>

```
$ kubectl get events
```

<br/>

```
$ cd ~/projects/dev/ml/
$ git clone https://github.com/webmakaka/Machine-Learning-on-Kubernetes.git
$ cd ~/projects/dev/ml/Machine-Learning-on-Kubernetes
$ git switch main
```

<br/>

```
// $ kubectl delete -f Chapter04/catalog-source.yaml
$ kubectl create -f Chapter04/catalog-source.yaml
```

<br/>

```
// $ kubectl describe catalogsource community-operators-redhat -n olm
$ kubectl describe catalogsource operatorhubio-catalog -n olm
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
opendatahub-operator                        Community Operators Red Hat   52s
```

<br/>

```
$ kubectl create -f Chapter04/odh-subscription.yaml
$ watch kubectl get subscription -n operators
```
