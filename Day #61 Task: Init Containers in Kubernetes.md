# Day #61 task: Init Containers in Kubernetes
There are some applications that need to be deployed on Kubernetes cluster and these apps have some pre-requisites where some configurations need to be changed before deploying the app container. Some of these changes cannot be made inside the images so the DevOps team has come up with a solution to use init containers to perform these tasks during deployment. Below is a sample scenario that the team is going to test first.

1. Create a Deployment named as ic-deploy-xfusion.

2. Configure spec as replicas should be 1, labels app should be ic-xfusion, template's metadata lables app should be the same ic-xfusion.

3. The initContainers should be named as ic-msg-xfusion, use image fedora with latest tag and use command '/bin/bash', '-c' and 'echo Init Done - Welcome to xFusionCorp Industries > /ic/news'. The volume mount should be named as ic-volume-xfusion and mount path should be /ic.

4. Main container should be named as ic-main-xfusion, use image fedora with latest tag and use command '/bin/bash', '-c' and 'while true; do cat /ic/news; sleep 5; done'. The volume mount should be named as ic-volume-xfusion and mount path should be /ic.

5. Volume to be named as ic-volume-xfusion and it should be an emptyDir type.

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.



***Note**
Init containers are specialized containers that run to completion before the main application containers start inside a Kubernetes Pod. They are commonly used by DevOps teams to perform initialization, dependency checks, or setup tasks that shouldn't or cannot be baked into the main application image

## Step-1: Create the deployment manifest file with the given information.

Before we jump into creating the manifest, let’s just quickly note down the given information:
- Deployment name: ic-deploy-xfusion
- Replicas: 1
- Label: “app: ic-xfusion”

- Init Container Details:
~ name: ic-msg-xfusion
~ image: fedora:latest
~ command: ‘/bin/bash’, ‘-c’, and ‘echo Init Done — Welcome to xFusionCorp Industries > /ic/news’
~ Volume mounts name: ic-volume-xfusion
~ Mount Path: /ic

- Main Container Details:
~ name: ic-main-xfusion
~ image: fedora:latest
~ command: ‘/bin/bash’, ‘-c’ and ‘while true; do cat /ic/ecommerce; sleep 5; done’
~ Volume mounts name: ic-volume-xfusion
~ Mount Path: /ic

- Volume Details:
~ name: ic-volume-nautilus
~ type: emptyDir

Now that we’ve clearly listed out the details, let’s create the deployment manifest file, using them.

```
vi deployment.yaml
```
<img width="414" height="132" alt="image" src="https://github.com/user-attachments/assets/93bb86ea-2584-4c72-896b-c7f34184b45b" />


```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ic-deploy-xfusion
  labels:
    app: ic-xfusion
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ic-xfusion
  template:
    metadata:
      labels:
        app: ic-xfusion
    spec:
      # Init Container: Runs first to create the configuration file
      initContainers:
      - name: ic-msg-xfusion
        image: fedora:latest
        command: ['/bin/bash', '-c']
        args: ['echo Init Done - Welcome to xFusionCorp Industries > /ic/news']
        volumeMounts:
        - name: ic-volume-xfusion
          mountPath: /ic

      # Main Container: Starts only after the Init Container successfully completes
      containers:
      - name: ic-main-xfusion
        image: fedora:latest
        command: ['/bin/bash', '-c']
        args: ['while true; do cat /ic/news; sleep 5; done']
        volumeMounts:
        - name: ic-volume-xfusion
          mountPath: /ic

      # Volume: Provides shared storage for the setup file
      volumes:
      - name: ic-volume-xfusion
        emptyDir: {}
```
<img width="563" height="501" alt="image" src="https://github.com/user-attachments/assets/ab69f2c4-1b00-44c6-869c-41084a1b6bf9" />


## Step-2: Create the deployment using the given manifest file and validate the same.

```
kubectl apply -f deployment.yaml
```
```
kubectl get deploy
```
```
kubectl describe deployments.apps
```
<img width="732" height="501" alt="image" src="https://github.com/user-attachments/assets/747de9b1-a5c2-426d-be99-4201e60af2d9" />

<img width="719" height="379" alt="image" src="https://github.com/user-attachments/assets/1949ade8-db0e-4ac5-9b16-b740d98d8f42" />


## Step-3: Validate the pod and the container details.

```
kubectl get po
```

```
kubectl describe po
```
<img width="666" height="451" alt="image" src="https://github.com/user-attachments/assets/7c003ee1-3c32-4342-a7ab-75f534d4d4ef" />

<img width="660" height="529" alt="image" src="https://github.com/user-attachments/assets/d6b9cb0c-c782-4a72-af49-5933569b4830" />

<img width="831" height="330" alt="image" src="https://github.com/user-attachments/assets/f5a497b1-d51b-45ca-ae85-663a97c38ec0" />

## Step-4: Verify the logs.

```
kubectl logs ic-deploy-xfusion-9fc55f9cf-hhv7n
```
<img width="650" height="443" alt="image" src="https://github.com/user-attachments/assets/9890207a-da98-471f-af72-e0eac1f1b389" />


<img width="719" height="82" alt="image" src="https://github.com/user-attachments/assets/cdae5877-5119-412d-9083-b34dd06d2864" />
