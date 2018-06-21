# Deployment
kubernetes cli(kubectl) is used to create/manage deployment on a k8s Cluster.

Information provided during deployment:
 - Container image
 - Number of replicas
 -

Commands to play with

```
Create deployment
kubectl run NAME --image=image [--env="key=value"] [--port=port] [--replicas=replicas] [--dry-run=bool]
[--overrides=inline-json] [--command] -- [COMMAND] [args...] [options]
```

```
Get deployment info
kubectl get
[(-o|--output=)json|yaml|wide|custom-columns=...|custom-columns-file=...|go-template=...|go-template-file=...|jsonpath=...|jsonpath-file=...]
(TYPE [NAME | -l label] | TYPE/NAME ...) [flags] [options]
```

```
Describe deployment
kubectl describe (-f FILENAME | TYPE [NAME_PREFIX | -l label] | TYPE/NAME) [options]
```

Example
```
kubectl run sandyot-nginx --image=sandyot/nginx --port=80
deployment "sandyot-nginx" created
```

```
kubectl get -o wide deploy
NAME            DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE       CONTAINERS      IMAGES          SELECTOR
sandyot-nginx   1         1         1            1           3m        sandyot-nginx   sandyot/nginx   run=sandyot-nginx
```

```
kubectl describe deploy sandyot-nginx
Name:                   sandyot-nginx
Namespace:              default
CreationTimestamp:      Fri, 22 Jun 2018 00:35:12 +0800
Labels:                 run=sandyot-nginx
Annotations:            deployment.kubernetes.io/revision=1
Selector:               run=sandyot-nginx
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  1 max unavailable, 1 max surge
Pod Template:
  Labels:  run=sandyot-nginx
  Containers:
   sandyot-nginx:
    Image:        sandyot/nginx
    Port:         80/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   sandyot-nginx-7f95b6c649 (1/1 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  5m    deployment-controller  Scaled up replica set sandyot-nginx-7f95b6c649 to 1
```
