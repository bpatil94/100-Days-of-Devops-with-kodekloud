# Day #51 task: Execute Rolling Updates in Kubernetes

An application currently running on the Kubernetes cluster employs the nginx web server. The Nautilus application development team has introduced some recent changes that need deployment. They've crafted an image nginx:1.17 with the latest updates.

Execute a rolling update for this application, integrating the nginx:1.17 image. The deployment is named nginx-deployment.

Ensure all pods are operational post-update.

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.



## step 1: Checked the current state of the deployment
```
kubectl get deployments
```
```
kubectl get pods
```
<img width="650" height="436" alt="image" src="https://github.com/user-attachments/assets/b9a7ebb6-4455-4eda-9e4a-43664a10a4c2" />

or 
```
kubectl get all
```
- check the current version of nginx

```
kubectl describe deployment nginx-deployment
```
- see under container section for image version

  <img width="679" height="452" alt="image" src="https://github.com/user-attachments/assets/bb203750-267d-4b6b-909a-edf39651aecc" />

## step 2: Updated the deployment to use the new image:
```
kubectl edit deployment nginx-deployment
```
- change the image to nginx:1.17 under the container section 
  
<img width="627" height="35" alt="image" src="https://github.com/user-attachments/assets/138b2563-d4de-4c97-b9db-1f65f39634d3" />

or you can use this

```
kubectl set image deployment/nginx-deployment nginx-container=nginx:1.17
```
## step 3:  Verified the rolling update progress
```
kubectl rollout status deployment/nginx-deployment
```
<img width="542" height="34" alt="image" src="https://github.com/user-attachments/assets/304c89be-38d1-45ab-9bb5-0a992d953540" />

```
kubectl get pods
```
<img width="684" height="288" alt="image" src="https://github.com/user-attachments/assets/d0bc5953-8f00-4f34-a4f7-d83c29141700" />

- old replica sets are zero

<img width="674" height="335" alt="image" src="https://github.com/user-attachments/assets/e5a5242b-dc67-4735-a0ac-8e26c0886e3e" />



