# Open Data Hub (ODH) operator installation

https://github.com/opendatahub-io/opendatahub-operator

<br/>

// looks this method is not working right now  
https://dev.operatorhub.io/operator/opendatahub-operator

<br/>

```yaml
$ cat << EOF | kubectl apply -f -
apiVersion: operators.coreos.com/v1alpha1
kind: CatalogSource
metadata:
  name: community-operators-redhat
  namespace: olm
spec:
  displayName: Community Operators Red Hat
  #  image: registry.access.redhat.com/redhat/community-operator-index:v4.13
  image: registry.access.redhat.com/redhat/community-operator-index:v4.9
  publisher: RedHat
  sourceType: grpc
  updateStrategy:
    registryPoll:
      interval: 60m
EOF
```

<br/>

```
// wait 2-4 minutes
$ kubectl get packagemanifests -o wide -n olm | grep -I opendatahub
opendatahub-operator                        Community Operators Red Hat   52s
```

<br/>

```yaml
$ cat << EOF | kubectl apply -f -
apiVersion: operators.coreos.com/v1alpha1
kind: Subscription
metadata:
  name: opendatahub-operator
  namespace: operators
spec:
  channel: stable
  installPlanApproval: Automatic
  name: opendatahub-operator
  source: community-operators-redhat
  sourceNamespace: olm
  startingCSV: opendatahub-operator.v1.1.1
EOF
```

<br/>

```
$ watch kubectl get subscription -n operators
NAME                   PACKAGE                SOURCE                       CHANNEL
opendatahub-operator   opendatahub-operator   community-operators-redhat   stable
```

<br/>

```
$ kubectl describe catalogsource community-operators-redhat -n olm
$ kubectl describe catalogsource operatorhubio-catalog -n olm
```

<br/>

```
// 120 sec
$ watch kubectl get pods -n operators
NAME                                   READY   STATUS    RESTARTS   AGE
opendatahub-operator-b5f4c5757-d9td2   1/1     Running   0          15s
```
