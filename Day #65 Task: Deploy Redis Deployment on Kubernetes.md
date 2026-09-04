# Day #65 task: Deploy Redis Deployment on Kubernetes
The Nautilus application development team observed some performance issues with one of the application that is deployed in Kubernetes cluster. After looking into number of factors, the team has suggested to use some in-memory caching utility for DB service. After number of discussions, they have decided to use Redis. Initially they would like to deploy Redis on kubernetes cluster for testing and later they will move it to production. Please find below more details about the task:

Create a redis deployment with following parameters:

1. Create a config map called my-redis-config having maxmemory 2mb in redis-config.

2. Name of the deployment should be redis-deployment, it should use
redis:alpine image and container name should be redis-container. Also make sure it has only 1 replica.

3. The container should request for 1 CPU.

4. Mount 2 volumes:

a. An Empty directory volume called data at path /redis-master-data.

b. A configmap volume called redis-config at path /redis-master.

c. The container should expose the port 6379.

5. Finally, redis-deployment should be up and running.

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.


## Step-1: Config-map creation using manifest with the given details from problem statement.
As per the problem statement, we need to create a config-map with the following details:
- Name of the configmap: my-redis-config
- Key: redis-config
- Value: maxmemory 2mb


```
vi config.yaml
```

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-redis-config
data:
  redis-config: |
    maxmemory 2mb
```
<img width="217" height="124" alt="image" src="https://github.com/user-attachments/assets/366ee1b2-76bc-4bd1-9473-c7739254af93" />


**Explanation**
- apiVersion: v1 → ConfigMap uses Kubernetes core API v1.
- kind: ConfigMap → Creates a ConfigMap.
- name: my-redis-config → Name of the ConfigMap.
- data → Contains configuration data.
- redis-config → The name/key of the configuration file.
- maxmemory 2mb → Redis will use a maximum of 2 MB memory.


## step2: apply config.yaml and describe that
```
kubectl apply -f config.yaml
```
```
kubectl describe cm my-redis-config
```

<img width="386" height="491" alt="image" src="https://github.com/user-attachments/assets/c98d1e42-bfb3-4f49-a258-ed6adc447a19" />

## Step-3: Deployment creation using manifest with the given details from problem statement.

As per the problem statement, we need to create a deployment with the following details:

- Name of the deployment: redis-deployment
- Name of the image: redis:alpine
- Name of the container: redis-container
- Replica: 1
- Container Port: 6379
- Request: 1 CPU
- Volumes (2):
— Name: data
— Type: Empty Directory.
— Mount Path: /redis-master-data
— Name: redis-config
— Type: Config Map.
— Mount Path: /redis-master

```
vi deployment.yaml
```
```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: redis-deployment
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis-deployment
  strategy: {}
  template:
    metadata:
      labels:
        app: redis-deployment
    spec:
      volumes:
        - name: data
          emptyDir: {}
        - name: redis-config
          configMap:
            name: my-redis-config
      containers:
      - image: redis:alpine
        name: redis-container
        ports:
        - containerPort: 6379
        resources: 
         requests:
          cpu: "1"
        volumeMounts:
        - name: data
          mountPath: /redis-master-data
        - name: redis-config
          mountPath: /redis-master
status: {}
```

<img width="585" height="492" alt="image" src="https://github.com/user-attachments/assets/d79451c5-a610-4a6b-99dd-691290e96bb7" />

<img width="462" height="250" alt="image" src="https://github.com/user-attachments/assets/9e16d77b-c28e-49b2-afaa-f01cdd52fce7" />


## step4: apply and describe the deployment

```
k apply -f deployment.yaml
```
```
k get pods
```
<img width="653" height="275" alt="image" src="https://github.com/user-attachments/assets/cb3fc6af-21ee-423e-842c-7330e09b4c3c" />

```
k describe deploy
```
<img width="674" height="470" alt="image" src="https://github.com/user-attachments/assets/bddd81bc-98b8-4753-a734-5c719124c58c" />


<img width="978" height="293" alt="image" src="https://github.com/user-attachments/assets/69f59dd3-e274-4415-8482-7061e8a2d429" />

```
k describe po
```
<img width="858" height="444" alt="image" src="https://github.com/user-attachments/assets/0aef7705-20e9-495b-a6be-3fbd4595c8df" />

<img width="861" height="423" alt="image" src="https://github.com/user-attachments/assets/83260f01-3981-48d9-ba8e-205ea3c97576" />

<img width="1109" height="290" alt="image" src="https://github.com/user-attachments/assets/ff26c76a-f0cc-4be5-b22c-39977a27e101" />




