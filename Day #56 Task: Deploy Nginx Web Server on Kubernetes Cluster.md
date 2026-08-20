# Day #56 task: Deploy Nginx Web Server on Kubernetes Cluster
Some of the Nautilus team developers are developing a static website and they want to deploy it on Kubernetes cluster. They want it to be highly available and scalable. Therefore, based on the requirements, the DevOps team has decided to create a deployment for it with multiple replicas. Below you can find more details about it:


Create a deployment using nginx image with latest tag only and remember to mention the tag i.e nginx:latest. Name it as nginx-deployment. The container should be named as nginx-container, also make sure replica counts are 3.

Create a NodePort type service named nginx-service. The nodePort should be 30011.

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.


# step1: check kubectl version and present deployments
```
kubectl version
```
```
kubectl get all
```

# step2: Create deployment.yaml and service.yaml file as specified in task description
```
vi deployment.yaml
```
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx-container
          image: nginx:latest
          ports:
            - containerPort: 80
```
<img width="355" height="308" alt="image" src="https://github.com/user-attachments/assets/9d8cd526-6827-4393-80d4-3055670608d3" />


```
vi service.yaml
```
```
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30011
```
<img width="288" height="214" alt="image" src="https://github.com/user-attachments/assets/cd50a8e3-78bd-484d-94dd-a917dbf47b0a" />

```
ls
```
<img width="450" height="96" alt="image" src="https://github.com/user-attachments/assets/55759990-ac6f-4dee-b4f5-98cecb7b6670" />


# step3: Apply the configuration created above
```
kubectl apply -f deployment.yaml
```
```
kubectl apply -f service.yaml
```
```
kubectl get all
```
<img width="668" height="341" alt="image" src="https://github.com/user-attachments/assets/3b5010e5-5f2b-46b0-b50c-8894c3945e2d" />

# step4: display the pods names which Selects Pods whose labels contain:
```
kubectl get pods -l app=nginx
```

# step5: access the nginx page from jump-host
```
curl http://localhost:30011
```
<img width="581" height="450" alt="image" src="https://github.com/user-attachments/assets/48e10cb5-ea0c-4b89-9608-bf6287ee53b0" />

 
