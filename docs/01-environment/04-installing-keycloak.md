# Keycloak installation

<br/>

```
$ cd ~/projects/dev/ml/
// $ git clone https://github.com/webmakaka/Machine-Learning-on-Kubernetes.git
$ git clone git@github.com:webmakaka/Machine-Learning-on-Kubernetes.git
$ cd ~/projects/dev/ml/Machine-Learning-on-Kubernetes
$ git switch main
```

<br/>

```
$ kubectl create ns keycloak
$ kubectl create -f Chapter04/postgresdb/ -n keycloak
```

<br/>

```
// 30 sec
$ watch kubectl get pods -n keycloak
NAME                        READY   STATUS    RESTARTS   AGE
postgres-655d75f54b-xzvh6   1/1     Running   0          30s
```

<br/>

```
$ kubectl create -f Chapter04/keycloak/keycloak.yaml -n keycloak
```

<br/>

```
// 60 sec
$ watch kubectl get pods -n keycloak
NAME                        READY   STATUS    RESTARTS   AGE
keycloak-75799d947b-l9mkw   1/1     Running   0          56s
postgres-9db8ff595-vc8x2    1/1     Running   0          2m7s

```

<!--

<br/>

```
$ minikube ip --profile ${PROFILE}
192.168.49.2
```

<br/>

```
$ export MINIKUBE_IP_ADDR=192.168.49.2
```

-->

<br/>

```
$ export MINIKUBE_IP_ADDR=$(minikube ip --profile ${PROFILE})
$ echo ${MINIKUBE_IP_ADDR}
```

<br/>

```
// Check
// $ envsubst < Chapter04/keycloak/keycloak-ingress.yaml
```

<br/>

```
$ envsubst < Chapter04/keycloak/keycloak-ingress.yaml | kubectl create -f - -n keycloak
```

<br/>

```
$ kubectl get ingress -n keycloak
NAME       CLASS   HOSTS                          ADDRESS        PORTS     AGE
keycloak   nginx   keycloak.192.168.49.2.nip.io   192.168.49.2   80, 443   4m8s
```

<br/>

```
// Administration Console
// admin / admin
$ echo https://keycloak.${MINIKUBE_IP_ADDR}.nip.io/auth/
```

<br/>

### Importing the Keycloak configuration for the ODH components

```
Keycloak WEB UI

import -> Chapter05/realm-export.json

- If a resource exists - Skip

Import
```

<br/>

### Creating a Keycloak user

<br/>

```
Users -> Add user

Username: mluser
Email: mluser@example.com
First Name: mluser
Last Name: mluser

User Enabled: ON
Email Verified: ON

Groups: ml-group

SAVE
```

<br/>

```
Credentials:

Password: mluser
Password Confirmation: mluser

Temporary: OFF

Set Password
```

<br/>

```
$ watch kubectl get pods -n operators
NAME                                   READY   STATUS    RESTARTS   AGE
opendatahub-operator-b5f4c5757-d9td2   1/1     Running   0          15s
```
