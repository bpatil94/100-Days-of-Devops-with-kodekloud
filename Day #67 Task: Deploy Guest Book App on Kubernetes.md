# Day #67 task: Deploy Guest Book App on Kubernetes
The Nautilus Application development team has finished development of one of the applications and it is ready for deployment. It is a guestbook application that will be used to manage entries for guests/visitors. As per discussion with the DevOps team, they have finalized the infrastructure that will be deployed on Kubernetes cluster. Below you can find more details about it.

***BACK-END TIER***

1. Create a deployment named redis-master for Redis master.

  a.) Replicas count should be 1.

  b.) Container name should be master-redis-datacenter and it should use image redis.

  c.) Request resources as CPU should be 100m and Memory should be 100Mi.

  d.) Container port should be redis default port i.e 6379.

2. Create a service named redis-master for Redis master. Port and targetPort should be Redis default port i.e 6379.

3. Create another deployment named redis-slave for Redis slave.

  a.) Replicas count should be 2.

  b.) Container name should be slave-redis-datacenter and it should use gcr.io/google_samples/gb-redisslave:v3 image.

  c.) Requests resources as CPU should be 100m and Memory should be 100Mi.

  d.) Define an environment variable named GET_HOSTS_FROM and its value should be dns.

  e.) Container port should be Redis default port i.e 6379.

4. Create another service named redis-slave. It should use Redis default port i.e 6379.

5. Create another service named redis-follower. Port and targetPort should be Redis default port i.e 6379. Its selector app should be redis-slave.

***FRONT END TIER***

1. Create a deployment named frontend.

  a.) Replicas count should be 3.

  b.) Container name should be php-redis-datacenter and it should use gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff image.

  c.) Request resources as CPU should be 100m and Memory should be 100Mi.

  d.) Define an environment variable named as GET_HOSTS_FROM and its value should be dns.

  e.) Container port should be 80.

2. Create a service named frontend. Its type should be NodePort, port should be 80 and its nodePort should be 30009.

Finally, you can check the guestbook app by clicking on App button.

You can use any labels as per your choice.

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.


# Step-1: Let’s create the manifest files for redis-master deployment and service and create the resources using them.

When we click on the app button, we will be taken to a 502 Bad Gateway error page as seen below. Let’s resolve it.


<img width="1364" height="352" alt="image" src="https://github.com/user-attachments/assets/0728ad49-9063-4e5b-a189-4a780f92c394" />




- #Create the manifest for the redis-master deployment for BACKEND-TIER.

```
vi backend-deploy.yaml
```
- insert the contents as per the description

```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: redis-master
  name: redis-master
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis-master
  strategy: {}
  template:
    metadata:
      labels:
        app: redis-master
    spec:
      containers:
      - image: redis:alpine
        name: master-redis-datacenter
        resources: 
          requests:
            cpu: "100m"
            memory: "100Mi"
        ports:
        - containerPort: 6379

```
<img width="507" height="449" alt="image" src="https://github.com/user-attachments/assets/92e4c7a9-e613-4489-9d97-17384bb5ce30" />

- Let's create the redis-master deployment.
```
kubectl apply -f backend-deploy.yaml
```
<img width="652" height="109" alt="image" src="https://github.com/user-attachments/assets/9ca86468-f7f8-4a25-856d-9ecf73698bea" />


- #Create the manifest for the redis-master service for BACKEND-TIER.

```
vi backend-svc.yaml 
```
- insert the content as per description
```
apiVersion: v1
kind: Service
metadata:
  name: redis-master
spec:
  selector:
    app: redis-master
  ports:
    - targetPort: 6379
      port: 6379

```
<img width="612" height="238" alt="image" src="https://github.com/user-attachments/assets/f237c6ca-d87e-4d37-9065-6fdebcc31bc3" />

- Let's create the redis-master service.

```
kubectl apply -f backend-svc.yaml 
```

<img width="607" height="111" alt="image" src="https://github.com/user-attachments/assets/a320abe3-5d3d-41c3-b641-833ffd136445" />


# Step-2: Let’s create the manifest files for redis-slave deployment and service and create the resources using them.

- #Create the manifest for the redis-slave deployment for BACKEND-TIER.
```
vi redis-slave-deploy.yaml
```
- inser the contents
```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: redis-slave
  name: redis-slave
spec:
  replicas: 2
  selector:
    matchLabels:
      app: redis-slave
  strategy: {}
  template:
    metadata:
      labels:
        app: redis-slave
    spec:
      containers:
      - image: gcr.io/google_samples/gb-redisslave:v3
        name: slave-redis-datacenter
        resources: 
          requests:
            cpu: "100m"
            memory: "100Mi"
        ports:
        - containerPort: 6379
        env:
        - name: GET_HOSTS_FROM
          value: "dns"
```
<img width="595" height="484" alt="image" src="https://github.com/user-attachments/assets/b90b960e-c7f6-4f9d-9e93-7c8dc67c5f52" />


- Let's create the redis-slave deployment.

```
kubectl apply -f redis-slave-deploy.yaml
```
<img width="710" height="133" alt="image" src="https://github.com/user-attachments/assets/5058833e-e7a5-4cdd-bbef-d88d6d158568" />


- #Create the manifest for the redis-slave service for BACKEND-TIER.

```
vi redis-slave-svc.yaml 
```
- insert the contents

```
apiVersion: v1
kind: Service
metadata:
  name: redis-slave
spec:
  selector:
    app: redis-slave
  ports:
    - targetPort: 6379
      port: 6379
```
<img width="251" height="226" alt="image" src="https://github.com/user-attachments/assets/b46bc57f-184a-4900-bf5e-d4bd4beda3c6" />

- Create another service named redis-follower
  
```
vi redis-follower-svc.yaml 
```
- insert the contents

```
apiVersion: v1
kind: Service
metadata:
  name: redis-follower
spec:
  selector:
    app: redis-slave
  ports:
    - targetPort: 6379
      port: 6379
```
<img width="354" height="249" alt="image" src="https://github.com/user-attachments/assets/e97ba34e-77c0-47c9-bd5b-d9a4b2d8775d" />


- Let's create the redis-slave and redis-follower service.
```
kubectl apply -f redis-slave-svc.yaml
```

```
kubectl apply -f redis-follower-svc.yaml
```
<img width="694" height="157" alt="image" src="https://github.com/user-attachments/assets/21ce3f68-0d6e-4e45-ace1-cf2216661b53" />


# Step-3: Let’s create the manifest files for frontend deployment and service and create the resources using them.
- #Create the manifest for the frontend deployment for FRONTEND-TIER.
  
```
vi frontend-deploy.yaml
```

- insert the contents

```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: frontend
  name: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend
  strategy: {}
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - image: gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff
        name: php-redis-datacenter
        resources: 
          requests:
            cpu: "100m"
            memory: "100Mi"
        env:
        - name: "GET_HOSTS_FROM"
          value: "dns"
        ports:
        - containerPort: 80

```
<img width="898" height="498" alt="image" src="https://github.com/user-attachments/assets/724bb2fc-02c0-46f6-8749-90f7ef378aa3" />


- Let's create the frontend deployment.

```
kubectl apply -f frontend-deploy.yaml
```
<img width="686" height="140" alt="image" src="https://github.com/user-attachments/assets/814277c9-cbac-459a-99ac-3ecf0ec346ad" />


- #Create the manifest for the frontend service for FRONTEND-TIER.

```
vi frontend-svc.yaml
```

```
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  type: NodePort
  selector:
    app: frontend
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30009
```
<img width="327" height="303" alt="image" src="https://github.com/user-attachments/assets/abafa5fc-4f31-4082-8746-7af55ed5bcf8" />


- Let's create the frontend service

```
kubectl apply -f frontend-svc.yaml
```
<img width="822" height="86" alt="image" src="https://github.com/user-attachments/assets/029feb61-78b3-4a48-8c41-641c829ffc57" />



# Step-4: Cross-verify all the details of the deployments and services created.
- Check if the deployments are created successfully.
```
k get deploy
```

- Check if the pods are in running state
```
k get pods
```
<img width="682" height="114" alt="image" src="https://github.com/user-attachments/assets/bd4b8a37-75f2-4eac-a1d0-f8e78496d28e" />


- Validate the details of the frontend deployment.
```
k describe deploy frontend
```
<img width="906" height="502" alt="image" src="https://github.com/user-attachments/assets/84cd72c2-83c4-44d2-878d-6a9b59820f25" />
<img width="911" height="187" alt="image" src="https://github.com/user-attachments/assets/10b67729-84cf-48a4-a127-005050fce05a" />


- Validate the details of the redis-master [BACKEND-TIER] deployment.
```
kubectl describe deploy redis-master
```
<img width="810" height="498" alt="image" src="https://github.com/user-attachments/assets/d83cbdd0-47a0-4fd2-a6ae-4ca3da76922e" /> 

<img width="906" height="182" alt="image" src="https://github.com/user-attachments/assets/f7aec062-0a51-48de-84c9-6d02b246843f" />


- Validate the details of the redis-slave [BACKEND-TIER] deployment.
```
kubectl describe deploy redis-slave
```
<img width="720" height="512" alt="image" src="https://github.com/user-attachments/assets/fdf2279b-260a-4b9f-8cb2-d2d7d4883eaf" />

<img width="904" height="180" alt="image" src="https://github.com/user-attachments/assets/922d5295-ed50-4b2d-8723-d2be334ed033" />

- see all created things
```
k get all
```

<img width="957" height="408" alt="image" src="https://github.com/user-attachments/assets/5b0d2fca-bb1d-412f-b654-e89e1303fac9" />


# step-5: Now let’s click on the app and see, if you can access the app.

<img width="1364" height="630" alt="image" src="https://github.com/user-attachments/assets/b5a23186-88c2-486f-9e0d-ae5bf59b303d" />



***Note**
- Analyze the ip ports and its mapping

<img width="1025" height="368" alt="image" src="https://github.com/user-attachments/assets/e05e4383-1c59-4eee-b6bb-649a1084a612" />


<img width="833" height="422" alt="image" src="https://github.com/user-attachments/assets/01abf1d9-0d59-436d-a44f-679b0049aa69" />

