# Service

- Kubernetes Pods are mortal.
- A Service in Kubernetes is an abstraction which defines a logical set of Pods and a policy by which to access them.
- A Service is defined using YAML/JSON, like all Kubernetes objects
- The set of Pods targeted by a Service is usually determined by a LabelSelector
Services match a set of Pods using labels and selectors
- Labels can be attached to objects at creation time or later on. They can be modified at any time

Services can be exposed in different ways by specifying a type in the ServiceSpec:
  - ClusterIP (default): Exposes the Service on an internal IP in the cluster. This type makes the Service only reachable from within the cluster.
  - NodePort - Exposes the Service on the same port of each selected Node in the cluster using NAT. Makes a Service accessible from outside the cluster using <NodeIP>:<NodePort>. Superset of ClusterIP
  - LoadBalancer - Creates an external load balancer in the current cloud (if supported) and assigns a fixed, external IP to the Service. Superset of NodePort.
  - ExternalName - Exposes the Service using an arbitrary name (specified by externalName in the spec) by returning a CNAME record with the name. No proxy is used. This type requires v1.7 or higher of kube-dns.

```
kubectl expose (-f FILENAME | TYPE NAME) [--port=port] [--protocol=TCP|UDP] [--target-port=number-or-name] [--name=name] [--external-ip=external-ip-of-service] [--type=type] [options]
```

```
kubectl get -o wide services
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE       SELECTOR
kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   59m       <none>
```

```
kexpose deployment/sandyot-nginx-def --type="NodePort"
error: couldn't find port via --port flag or introspection
See 'kubectl expose -h' for help and examples.

kexpose deployment/sandyot-nginx-def --type="NodePort" --target-port=80
error: couldn't find port via --port flag or introspection
See 'kubectl expose -h' for help and examples.

kexpose deployment/sandyot-nginx-def --type="NodePort" --port=80
service "sandyot-nginx-def" exposed

kexpose deployment/sandyot-nginx-def --type="NodePort" --port=8090 --target-port=80  --name=def-custom
service "def-custom" exposed

kexpose deployment/sandyot-nginx-http --type="NodePort"
service "sandyot-nginx-http" exposed

kexpose deployment/sandyot-nginx-https --type="NodePort" --target-port=80
service "sandyot-nginx-https" exposed

kexpose deployment/sandyot-nginx-web --type="NodePort" --target-port=80
service "sandyot-nginx-web" exposed

```

```
kget -o wide svc
NAME                  TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE       SELECTOR
def-custom            NodePort    10.43.248.230   <none>        8090:30114/TCP   15s       run=sandyot-nginx-def
kubernetes            ClusterIP   10.43.0.1       <none>        443/TCP          3h        <none>
sandyot-nginx-def     NodePort    10.43.178.202   <none>        80:30011/TCP     16s       run=sandyot-nginx-def
sandyot-nginx-http    NodePort    10.43.15.130    <none>        80:31296/TCP     14s       run=sandyot-nginx-http
sandyot-nginx-https   NodePort    10.43.29.109    <none>        443:30520/TCP    13s       run=sandyot-nginx-https
sandyot-nginx-web     NodePort    10.43.83.166    <none>        8080:31201/TCP   12s       run=sandyot-nginx-web

```

```
kdesc  svc
Name:                     def-custom
Namespace:                default
Labels:                   run=sandyot-nginx-def
Annotations:              field.cattle.io/publicEndpoints=[{"addresses":["104.131.165.15"],"port":30114,"protocol":"TCP","serviceName":"default:def-custom","allNodes":true}]
Selector:                 run=sandyot-nginx-def
Type:                     NodePort
IP:                       10.43.248.230
Port:                     <unset>  8090/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  30114/TCP
Endpoints:                10.42.1.4:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>

Name:                     sandyot-nginx-def
Namespace:                default
Labels:                   run=sandyot-nginx-def
Annotations:              field.cattle.io/publicEndpoints=[{"addresses":["104.131.165.15"],"port":30011,"protocol":"TCP","serviceName":"default:sandyot-nginx-def","allNodes":true}]
Selector:                 run=sandyot-nginx-def
Type:                     NodePort
IP:                       10.43.178.202
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  30011/TCP
Endpoints:                10.42.1.4:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>


Name:                     sandyot-nginx-http
Namespace:                default
Labels:                   run=sandyot-nginx-http
Annotations:              field.cattle.io/publicEndpoints=[{"addresses":["104.131.165.15"],"port":31296,"protocol":"TCP","serviceName":"default:sandyot-nginx-http","allNodes":true}]
Selector:                 run=sandyot-nginx-http
Type:                     NodePort
IP:                       10.43.15.130
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  31296/TCP
Endpoints:                10.42.2.4:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>


Name:                     sandyot-nginx-https
Namespace:                default
Labels:                   run=sandyot-nginx-https
Annotations:              field.cattle.io/publicEndpoints=[{"addresses":["104.131.165.15"],"port":30520,"protocol":"TCP","serviceName":"default:sandyot-nginx-https","allNodes":true}]
Selector:                 run=sandyot-nginx-https
Type:                     NodePort
IP:                       10.43.29.109
Port:                     <unset>  443/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  30520/TCP
Endpoints:                10.42.0.8:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>


Name:                     sandyot-nginx-web
Namespace:                default
Labels:                   run=sandyot-nginx-web
Annotations:              field.cattle.io/publicEndpoints=[{"addresses":["104.131.165.15"],"port":31201,"protocol":"TCP","serviceName":"default:sandyot-nginx-web","allNodes":true}]
Selector:                 run=sandyot-nginx-web
Type:                     NodePort
IP:                       10.43.83.166
Port:                     <unset>  8080/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  31201/TCP
Endpoints:                10.42.1.5:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```
## Summary
port=The port that the service should serve on. Copied from the resource being exposed, if unspecified
target-port/container-port=
