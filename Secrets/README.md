### Secrets Example

#### Create a secret which has a apssword encoded
```
kubectl create -f my-secret-password.yaml
```

#### Create a Pod which consumes this Secret as a Volume
```
kubectl create -f busybox-pod.yaml
```

#### Check everything is ok
```
kubectl exec sleeping-pod -ti -- cat /my_secret_password/password
```
> Output should be rootroot
