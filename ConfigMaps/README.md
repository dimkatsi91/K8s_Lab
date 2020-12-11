## ConfigMap Examples
--------------------------------------------------------------------------------------------
1. my-pod.yaml: consumes configmap as a volume

2. my-pod-ii.yaml: consumes configmap as an environmental variable
---------------------------------------------------------------------------------------------

#### Create the ConfigMap Object
```
kubectl create -f my-team-cm.yaml
```

#### Create the 1st Pod with busybox image and mount this ConfigMap object as a Volume
```
kubectl create -f my-po-cm.yaml
```

#### Ensure it is really in place
```
kubectl exec sleeping-pod -ti -- ls /etc/my_team
kubectl exec sleeping-pod -ti -- /bin/sh    # And then inspect folder /etc/my_team files .. 
```

### 2nd Example: Create the 2nd Pod with busybox image and use the created configmap as an env variable


```
$ kubectl create -f my-pod-ii.yaml
$ kubectl get po -o wide
$ kubectl exec sleeping-pod -ti -- /bin/sh -c 'env | grep team'
team.name=Olympiakos
team.town=Piraeus
team.year=1925
team.country=Greece
```
