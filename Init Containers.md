# Using Init Containers in Kubernetes
## Introduction
Init containers are a great way to customize container startup. This lab will allow you to test your knowledge of init containers by using them to solve problems in an existing Kubernetes cluster.

## Solution
Log in to the provided lab server using the credentials provided:
```
ssh cloud_user@<PUBLIC_IP_ADDRESS>
```
### Create a Sample Pod That Uses an Init Container to Delay Startup
Open the pod descriptor file:
```
vi pod.yml
```
Add an init container (at the same level as containers in the file) to delay startup until the shipping-svc service is available:
```
spec:
  ...
  initContainers:
  - name: shipping-svc-check
    image: busybox:1.27
    command: ['sh', '-c', 'until nslookup shipping-svc; do echo waiting for shipping-svc; sleep 2; done']
 ```
Save and exit the file by pressing Escape followed by :wq.

Create the pod:
```
kubectl create -f pod.yml
```
Check the status of the pod:
```
kubectl get pods
```
It should remain in the Init status.

### Test Your Setup by Creating the Service and Verifying the Pod Starts Up
Create the service from the shipping-svc.yml file:
```
kubectl create -f shipping-svc.yml
```
Check the status of your pod again:
```
kubectl get pods
```
It should enter the Running status after about a minute.

## Conclusion
Congratulations on successfully completing this hands-on lab!
