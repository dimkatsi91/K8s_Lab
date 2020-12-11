## Deployment Example & Info

- Create an nginx deployment yaml manifest/definition file  via next kubectl command, 
  and then create the actual deployment with kubectl create -f <file>.yaml command.
  This deployment shall have 3 replicas, nginx image v1.18 and a strategy of rollingUpdate
  specified later with strategy property under spec section of yaml file.

```
kubectl create deploy nginx-deployment --image=nginx:1.18 --replicas=3 --dry-run=client -o yaml > nginx-deployment.yaml
nano nginx-deployment.yaml ## and add strategy as rollingUpdate with two more specs -- check file for more info ..
kubectl create -f nginx-deployment.yaml
kubectl get deploy,po -o wide
watch 'kubectl get pods -o wide'
kubectl describe deploy nginx-deployment
```

- After creating the deployment, update it by seting a specific nginx image version.
  Then check rollout history fo this deployment, and then go back to first version 
  because v1.21 does not exist ... !

```
~> kubectl set image deployment/nginx-deployment nginx=nginx:1.21 --record
deployment.apps/nginx-deployment image updated
~> k get po -o wide 
~> k get po -o wide 
~> kubectl rollout history deployment/nginx-deployment
deployment.apps/nginx-deployment 
REVISION  CHANGE-CAUSE
1         <none>
2         kubectl set image deployment/nginx-deployment nginx=nginx:1.21 --record=true

~> kubectl rollout undo deployment/nginx-deployment
deployment.apps/nginx-deployment rolled back
~> k get po -o wide 
~> k get po -o wide 
NAME                                READY   STATUS    RESTARTS   AGE    IP          NODE         NOMINATED NODE   READINESS GATES
nginx-deployment-5fcb5ffc4d-4zvp2   1/1     Running   0          16s    10.44.0.5   kubenode01   <none>           <none>
nginx-deployment-5fcb5ffc4d-wc4pp   1/1     Running   0          16s    10.44.0.6   kubenode01   <none>           <none>
nginx-deployment-5fcb5ffc4d-wdpnb   1/1     Running   0          108s   10.44.0.3   kubenode01   <none>           <none>
```
