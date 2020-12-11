## Pods Examples

1. nginx-une.yaml: Creates a Pod which only has one container, using as image nginx:latest and a label type: webserver.

2. multiple.yaml: Creates a Pod which has a Pod that has #2 containers inside: nginx container & fluentd container which 
   can be used for logging purposes, as a Sidecar container for example. The nginx named Pod also has a label of type: webserver
   and a port for the first nginx container set to containerPort: 80.

- More info regarding the second container can be seen via next kubectl commands:

```
kubectl get po -o wide
kubectl describe po nginx
```

- Also, the contents of the 1st nginx container can be retrieved by the worker node it is deployed by issuing a command similar to the following:

```
curl http://10.44.0.1
```

- Have in mind this content is available from the host it is deployed. In order it to be available outside the cluster, a Service should be deployed
  and it should be attached/connected to the respective Pod by setting a 'selector' property under its spec section of the service manifets yaml file.


3. secure-box.yaml: Creates a pod with a container using the image: busybox which runs a command: sleep "3600", runs also as user with ID: 1000,
   group with ID: 2000 and has also a SecurityContext, which means this container of the Pod runs this user (1000) and this group (2000), and not as root user.
   Last, but not least, this busybox container of this sleeping-pod has a ReadinessProbe, which means that it will be set to READY status (kubectl get po) when
   this command is executed and the return/result code is ok, i.e. '0' (zero). 

- Run the following commands from master node in order to verify everything:
```
kubectl create -f secure-box.yaml
kubectl get po -o wide
kubectl describe po sleeping-pod
kubectl exec sleeping-pod -ti -- id
```
