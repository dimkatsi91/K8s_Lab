## Jobs & CronJobs Examples
--------------------------------------------------------------------------------------------
1. Create a Job using a debian image that runs a sleep 3600 command with a backoffLimit and
   an activeDeadlineSeconds preporties
2. Create the respective Cron Job using a deian image that runs the same sleep command and
   is executed every 1 min, i.e. --schedule="*/1 * * * *"
--------------------------------------------------------------------------------------------

### kubectl commands to create the template of these Job and Cronjob K8s Objects
- Create the Job:
```
kubectl create job sleeping-job --image=debian --dry-run=client -o yaml > sleeping-job.yaml
```

- Create the Cron Job:
```
kubectl create cronjob sleeping-job --schedule="*/1 * * * *" --image=debian --dry-run=client -o yaml > sleeping-cronjob.yaml
```

- Then edit the two created files and add the backoffLimit & activeDeadlineSeconds properties

### Create the Job and check what is going on:
```
kubectl get po -o wide --watch
NAME                 READY   STATUS              RESTARTS   AGE   IP       NODE         NOMINATED NODE   READINESS GATES
sleeping-job-wbdp5   0/1     ContainerCreating   0          14s   <none>   kubenode01   <none>           <none>
sleeping-job-wbdp5   1/1     Running             0          43s   10.44.0.1   kubenode01   <none>           <none>
sleeping-job-wbdp5   1/1     Terminating         0          45s   10.44.0.1   kubenode01   <none>           <none>

k get jobs -o wide
NAME           COMPLETIONS   DURATION   AGE     CONTAINERS      IMAGES   SELECTOR
sleeping-job   0/1           2m51s      2m51s   sleeping-cont   debian   controller-uid=b6feca47-8716-46e2-ae3f-d5c63bd287d1
```

### Create the Cron Job and check what is going on:
```
k create -f sleeping-cronjob.yaml
cronjob.batch/sleeping-job created

k get cronjobs -o wide
NAME           SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE   CONTAINERS           IMAGES   SELECTOR
sleeping-job   */1 * * * *   False     0        <none>          7s    sleeping-container   debian   <none>

k get po -o wide
NAME                            READY   STATUS      RESTARTS   AGE   IP          NODE         NOMINATED NODE   READINESS GATES
sleeping-job-1607029800-tvnzp   0/1     Completed   0          8s    10.44.0.1   kubenode01   <none>           <none>

```
