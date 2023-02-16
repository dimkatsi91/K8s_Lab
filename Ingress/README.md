# Apply the pod

```kubectl apply -f deployment.yaml```

# Apply the service

```kubectl apply -f service.yaml```

# Check nginx welcome page

```
kubectl get svc nginx
NAME    TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
nginx   NodePort   10.99.227.58   <none>        80:30088/TCP   12m

curl $(minikube ip):30088
```

# Apply the ingress

```kubectl apply -f ingress.yaml```

# Add info under /etc/hosts file in the form of : $(minikube ip) <host-name>

```
k get ing
NAME            CLASS   HOSTS        ADDRESS          PORTS   AGE
nginx-ingress   nginx   nginx.info   192.168.59.100   80      97s

echo "$(minikube ip) nginx.info" >> /etc/hosts

curl nginx.info
```