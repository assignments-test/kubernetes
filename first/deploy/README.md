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

Here port signify the port which will be exposed at pod level
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
kubectl run sandyot-nginx-https --image=sandyot/nginx --port=80

kubectl run sandyot-nginx-http --image=sandyot/nginx --port=80

kubectl run sandyot-nginx-https --image=sandyot/nginx --port=443

kubectl run sandyot-nginx-web --image=sandyot/nginx --port=8080
```

```
kubectl get -o wide deploy
NAME                  DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE       CONTAINERS            IMAGES          SELECTOR
sandyot-nginx-def     1         1         1            1           11m       sandyot-nginx-def     sandyot/nginx   run=sandyot-nginx-def
sandyot-nginx-http    1         1         1            1           9m        sandyot-nginx-http    sandyot/nginx   run=sandyot-nginx-http
sandyot-nginx-https   1         1         1            1           8m        sandyot-nginx-https   sandyot/nginx   run=sandyot-nginx-https
sandyot-nginx-web     1         1         1            1           12s       sandyot-nginx-web     sandyot/nginx   run=sandyot-nginx-web

```

```
kdesc  deploy
Name:                   sandyot-nginx-def
Labels:                 run=sandyot-nginx-def
Annotations:            deployment.kubernetes.io/revision=1
Selector:               run=sandyot-nginx-def
Pod Template:
  Labels:  run=sandyot-nginx-def
  Containers:
   sandyot-nginx-def:
    Image:        sandyot/nginx
    Port:         <none>

Name:                   sandyot-nginx-http
Labels:                 run=sandyot-nginx-http
Annotations:            deployment.kubernetes.io/revision=1
Selector:               run=sandyot-nginx-http
Pod Template:
  Labels:  run=sandyot-nginx-http
  Containers:
   sandyot-nginx-http:
    Image:        sandyot/nginx
    Port:         80/TCP

Name:                   sandyot-nginx-https
Labels:                 run=sandyot-nginx-https
Annotations:            deployment.kubernetes.io/revision=1
Selector:               run=sandyot-nginx-https
Pod Template:
  Labels:  run=sandyot-nginx-https
  Containers:
   sandyot-nginx-https:
    Image:        sandyot/nginx
    Port:         443/TCP
```

```
kget -o  wide pods
NAME                                   READY     STATUS    RESTARTS   AGE       IP          NODE
sandyot-nginx-def-5bf887d9bd-lmqgz     1/1       Running   0          6m        10.42.1.4   worker2
sandyot-nginx-http-5f95cbb5cf-nmwg8    1/1       Running   0          4m        10.42.2.4   worker1
sandyot-nginx-https-57b7c8895d-pvgjs   1/1       Running   0          4m        10.42.0.8   master1
```
