# Day 58: Deploy Grafana on Kubernetes Cluster
The Nautilus DevOps teams is planning to set up a Grafana tool to collect and analyze analytics from some applications. They are planning to deploy it on Kubernetes cluster. Below you can find more details.

1.) Create a deployment named grafana-deployment-devops using any grafana image for Grafana app. Set other parameters as per your choice.

2.) Create NodePort type service with nodePort 32000 to expose the app.

You do not need to make any configuration changes inside the Grafana app once deployed; just make sure you can access the Grafana login page.

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.


# Step 1: Create the Deployment YAML
```
vi grafana-deployment.yaml
```
- insert the content
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana-deployment-devops
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      labels:
        app: grafana
    spec:
      containers:
        - name: grafana-container
          image: grafana/grafana:latest
          ports:
            - containerPort: 3000
```

<img width="431" height="324" alt="image" src="https://github.com/user-attachments/assets/6c381431-7924-49ed-b156-7e2c0feb349e" />


# Step 2: Create the NodePort Service YAML
```
vi grafana-service.yaml
```
- insert the content

```
apiVersion: v1
kind: Service
metadata:
  name: grafana-service
spec:
  type: NodePort
  selector:
    app: grafana
  ports:
    - port: 3000
      targetPort: 3000
      nodePort: 32000
```
<img width="332" height="247" alt="image" src="https://github.com/user-attachments/assets/d98edc14-f046-4d11-b57f-351bdfde491a" />


# Step 3: Apply Deployment and Service
```
kubectl apply -f grafana-deployment.yaml
```
```
kubectl apply -f grafana-service.yaml
```

- check the status

```
kubectl get deployments
```
kubectl get pods
```
```
kubectl get svc
```
<img width="731" height="259" alt="image" src="https://github.com/user-attachments/assets/4bcb1d10-6e4a-4911-bdef-419a12db4bf5" />


# Step 4: Access Grafana Login Page
Grafana login page should be accessible from jump-host.

<img width="424" height="46" alt="image" src="https://github.com/user-attachments/assets/f72d1745-9685-429f-8992-f75a8d12c0fd" />


- Grafana login page from pod

<img width="663" height="251" alt="image" src="https://github.com/user-attachments/assets/e863cee6-1c44-4995-b607-de94106f8fcb" />


