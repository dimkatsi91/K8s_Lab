## Services Examples

- ClusterIP: Allows reachability to the Pod nginx only within the cluster
- NodePort : Allows for mapping from a high port to the pod's internal port(80) 
             for reachability using Host's IP Address (curl <host-ip>:<high-port>)  
----------------------------------------------------------------------------------------

1. pod-une.yaml & svc-une.yaml: Creates an nginx pod with its container and a service 
   named nginx-nodeport-svc that creates a NodePort to map from a random high Port to the 
   internal port 80 of the nginx container, so this nginx container can be reached
   from the host that the K8s cluster and its worker node is deployed.

2. pod-une.yaml & svc-deux.yaml: Creates a Pod named nginx and a service named nginx-clusterip-svc.
   The service is of type ClusterIP which gives access to the nginx Pod only within the Cluster.

-----------------------------------------------------------------------------------------

NOTE-1: Create the pod via ```kubectl create -f po-une.yaml``` command

NOTE-2: Create the ClusterIP service with next command: ```kubectl create -f svc-deux.yaml```
        Or create the service with --dry-run argument and then edit it if needed, then create it:
        ``` kubectl expose po nginx --type=ClusterIP --port=80 --target-port=80 --name nginx --dry-run=client -o yaml > svc-deux.yaml ```
        And then edit it maybe and last ``` kubectl create -f svc-deux.yaml ```

NOTE-3: Same approach can be followed with the NodePort service, just change --type=NodePort and then
        edit the exported yaml file so a high port 'nodeport: xxxxx' is added in its manifest.

NOTE-4: After creating all these K8s Objects remove them all at once:
        ``` kubectl delete -f . ```

-----------------------------------------------------------------------------------------

Sample Output:
```
k get po -o wide --show-labels
NAME    READY   STATUS    RESTARTS   AGE   IP          NODE         NOMINATED NODE   READINESS GATES   LABELS
nginx   1/1     Running   0          17m   10.44.0.1   kubenode01   <none>           <none>            type=webserver
k get svc -o wide
NAME                  TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE   SELECTOR
kubernetes            ClusterIP   10.96.0.1       <none>        443/TCP        59d   <none>
nginx-clusterip-svc   ClusterIP   10.98.73.78     <none>        80/TCP         74s   type=webserver
nginx-nodeport-svc    NodePort    10.101.87.193   <none>        80:30080/TCP   7s    type=webserver
```
