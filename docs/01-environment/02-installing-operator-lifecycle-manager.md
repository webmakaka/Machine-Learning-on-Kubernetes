# Operator Lifecycle Manager (OLM) installation

<br/>

<!-- v0.23.1 -->

```
$ kubectl apply -f https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.19.1/crds.yaml
```

<br/>

```
$ kubectl apply -f https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.19.1/olm.yaml
```

<br/>

```
// 30 sec
$ watch kubectl get pods -n olm
NAME                               READY   STATUS    RESTARTS   AGE
catalog-operator-fc7fb45b5-mq9cw   1/1     Running   0          94s
olm-operator-765c45d458-f6rx2      1/1     Running   0          94s
operatorhubio-catalog-phnbt        1/1     Running   0          84s
packageserver-7bf4799ddf-7j99f     1/1     Running   0          84s
packageserver-7bf4799ddf-cfsdx     1/1     Running   0          84s
```

<br/>

```
// 45 sec
$ kubectl get catalogsource -n olm
NAME                    DISPLAY               TYPE   PUBLISHER        AGE
operatorhubio-catalog   Community Operators   grpc   OperatorHub.io   91s
```
