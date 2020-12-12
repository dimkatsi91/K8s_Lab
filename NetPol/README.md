## Network Policy very simple Example (Ingress type of traffic only)

- Create a deployment with 3 replicas and nginx image with label app: webserver
- Create a fluentd Pod with fluent image and label app: log
- Create Network Policy to allow Ingress traffic from fluent Pod
  to nginx Pods of nginx-deploy
  
```
k create -f .
```


```
k get po -o wide --show-labels
k get deploy -o wide --show-labels
k describe netpol net-pol-example

Name:         net-pol-example
Namespace:    default
Created on:   2020-12-12 11:52:16 +0200 EET
Labels:       <none>
Annotations:  <none>
Spec:
  PodSelector:     app=webserver
  Allowing ingress traffic:
    To Port: 6379/TCP
    From:
      PodSelector: app=log
  Not affecting egress traffic
  Policy Types: Ingress

```
