# Day 48: Deploy Pods in Kubernetes Cluster
The Nautilus DevOps team is diving into Kubernetes for application management. One team member has a task to create a pod according to the details below:

Create a pod named pod-httpd using the httpd image with the latest tag. Ensure to specify the tag as httpd:latest.

Set the app label to httpd_app, and name the container as httpd-container.

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.


- Infrastructure Details:

## step1: Check the kubectl is present or not
```
kubectl version
```
<img width="425" height="110" alt="image" src="https://github.com/user-attachments/assets/11e00d43-8819-424a-9f73-f4a4fa1f2bea" />


## step2: see any others pods running and see the present working namespace
```
kubectl get all
```
```
kubectl config get-contexts
```

<img width="548" height="161" alt="image" src="https://github.com/user-attachments/assets/3b18180b-81fa-44cb-80cd-d08fe4197c23" />


## step3: create the pod.yaml file and insert the contents
```
vi pod.yaml
```
```
apiVersion: v1
kind: Pod
metadata:
  name: pod-httpd
  labels:
    app: httpd_app
spec:
  containers:
  - name: httpd-container
    image: httpd:latest
```
<img width="345" height="177" alt="image" src="https://github.com/user-attachments/assets/81845852-bce0-4f55-93ba-175d4d080933" />


## step4: apply the kubectl command 

```
kubectl appy -f pod.yaml
```
- pod will get created in present namespace

<img width="576" height="233" alt="image" src="https://github.com/user-attachments/assets/9d798949-7751-4c55-9a13-3a01215f7190" />


