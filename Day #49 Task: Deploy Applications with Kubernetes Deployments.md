# Day 49: Deploy Applications with Kubernetes Deployments

The Nautilus DevOps team is delving into Kubernetes for app management. One team member needs to create a deployment following these details:


Create a deployment named nginx to deploy the application nginx using the image nginx:latest (ensure to specify the tag)

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.

-Infrastructure details: 


## step 1: check the kubectl version on jumphost, name space and Creating the Deployment
```
kubectl version
```
```
kubectl config get-contexts
```

<img width="392" height="117" alt="image" src="https://github.com/user-attachments/assets/521bc9da-7dfc-4ac7-b3f2-caf2a0d7b710" />

```
kubectl create deployment nginx --image=nginx:latest
```
<img width="656" height="194" alt="image" src="https://github.com/user-attachments/assets/4228604d-f662-4285-870f-a3d17e5ea45f" /> 

## step 2:  Verifying the Deployment
```
kubectl get deployments
```

<img width="419" height="98" alt="image" src="https://github.com/user-attachments/assets/3951e9e5-cf1d-4ead-acde-74fc7f900550" />


## step 3: Checking the Cluster Nodes
```
kubectl get nodes
```
<img width="580" height="46" alt="image" src="https://github.com/user-attachments/assets/35e4f1be-7808-461a-aa57-c25da485d249" />


## step 4: Conclusion

The deployment of the Nginx application was successful.
Kubernetes automatically created:

A Deployment to manage the application.
A ReplicaSet to maintain the desired number of pods.
A Pod running the nginx:latest container.
This task demonstrates the power of Kubernetes to deploy and manage applications with just a single command.
