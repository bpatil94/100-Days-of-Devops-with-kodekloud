# Day 52: Revert Deployment to Previous Version in Kubernetes
Earlier today, the Nautilus DevOps team deployed a new release for an application. However, a customer has reported a bug related to this recent release. Consequently, the team aims to revert to the previous version.

There exists a deployment named nginx-deployment; initiate a rollback to the previous revision.

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.


## step 1: Check the rollout history of the deployment:
```
kubectl rollout history deployment/nginx-deployment
```
<img width="671" height="354" alt="image" src="https://github.com/user-attachments/assets/7a664682-8253-45e9-ac04-2eb036da5b90" />

<img width="723" height="93" alt="image" src="https://github.com/user-attachments/assets/0e718382-fed9-4824-a68c-a1610bc22b01" />


## step 2:Rollback the deployment to the previous revision:
```
kubectl rollout undo deployment/nginx-deployment
```
<img width="530" height="36" alt="image" src="https://github.com/user-attachments/assets/3807243e-0494-4d9a-b520-5bbc971ccd3e" />

## step 3:Verify the rollout status and check pods:
```
kubectl get pods
```
<img width="730" height="167" alt="image" src="https://github.com/user-attachments/assets/41c4b04c-9936-44ba-b877-662e9f73feae" />


