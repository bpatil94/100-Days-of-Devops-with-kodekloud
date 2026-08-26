# Day #59 task: Troubleshoot Deployment issues in Kubernetes
Last week, the Nautilus DevOps team deployed a redis app on Kubernetes cluster, which was working fine so far. This morning one of the team members was making some changes in this existing setup, but he made some mistakes and the app went down. We need to fix this as soon as possible. Please take a look.

The deployment name is redis-deployment. The pods are not in running state right now, so please look into the issue and fix the same.

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.


## step 1: Check the existing deployments, pods, ConfigMap:
```
kubectl get deployments
```
```
kubectl get pods
```
```
kubectl get configmap
```
<img width="741" height="162" alt="image" src="https://github.com/user-attachments/assets/ccda83c4-d2ff-49da-8a47-6694de8ec184" />


- Get detailed information about the redis-deployment, including spec, status, and any events or errors:

  ```
  kubectl describe deployment redis-deployment
  ```
  <img width="889" height="482" alt="image" src="https://github.com/user-attachments/assets/e3b5ac17-16eb-4bdd-bb58-7e3e5f4a9644" />
  <img width="951" height="357" alt="image" src="https://github.com/user-attachments/assets/3d36cb2d-702e-4da8-9539-8279cba91e0a" />

- observe the below things
  <img width="617" height="356" alt="image" src="https://github.com/user-attachments/assets/af1e86df-366d-41bc-aef3-9a377db39cbf" />


- Describe the ConfigMap & pods:

  ```
  kubectl describe configmap
  ```
  <img width="530" height="341" alt="image" src="https://github.com/user-attachments/assets/d2cba450-89d6-4b82-a47f-73817ebcd727" />

  ```
  kubectl describe pods
  ```

## step 2 : Analyze errors to pinpoint issues like typos and Edit the deployment to fix errors:

```
kubectl edit deployment redis-deployment
```

- In the editor:
  1. Search for the misspelled word “cofig” (likely in a volumeMount or envFrom section referencing a ConfigMap) and correct it to “config”.
  2. Search for the misspelled word “alpin” (likely in the image field, e.g., redis:alpin) and correct it to “alpine” (e.g., redis:alpine).
  3. Save and exit the editor
  4. Kubernetes will automatically roll out the updated deployment and recreate pods.

<img width="729" height="241" alt="image" src="https://github.com/user-attachments/assets/d7512dc2-6376-4c32-9b6e-580701038332" />

  

## step 3 : Verify the deployment after fix:

```
kubectl get deployments
```
<img width="761" height="209" alt="image" src="https://github.com/user-attachments/assets/55be27dd-4e3d-4908-8883-ee6fb3424976" />


- observe the below image

  <img width="907" height="513" alt="image" src="https://github.com/user-attachments/assets/c3c46808-ccd4-4534-97b7-8940732e1bb3" />

```
kubectl get pods
```
<img width="709" height="52" alt="image" src="https://github.com/user-attachments/assets/1e0a2744-47ce-4f54-8879-780dee8fb064" />
