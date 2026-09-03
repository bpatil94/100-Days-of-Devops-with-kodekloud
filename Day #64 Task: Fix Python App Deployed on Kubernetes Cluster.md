# Day #64 task: Fix Python App Deployed on Kubernetes Cluster
One of the DevOps engineers was trying to deploy a python app on Kubernetes cluster. Unfortunately, due to some mis-configuration, the application is not coming up. Please take a look into it and fix the issues. Application should be accessible on the specified nodePort.

The deployment name is python-deployment-datacenter, its using poroko/flask-demo-app image. The deployment and service of this app is already deployed.

nodePort should be 32345 and targetPort should be python flask app's default port.

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.


## step1: Check the Current Deployment and Pod Status
```
k get all
```
<img width="739" height="278" alt="image" src="https://github.com/user-attachments/assets/2ede91aa-9b86-457a-87ac-d24df7dae204" />


```
kubectl get pod
```
<img width="770" height="56" alt="image" src="https://github.com/user-attachments/assets/104e6dcc-b499-4173-b475-986318729765" />

- observe the issue and solve the problem , by describing pod
  ```
  k describe pod python-deployment-datacenter
  ```
  <img width="664" height="505" alt="image" src="https://github.com/user-attachments/assets/d1832855-5baa-4d0c-a9fa-01ae2512f628" />


**Findings:**
The image name in the deployment is incorrect: poroko/flask-app-demo
Correct public image on Docker Hub: poroko/flask-demo-app

## step2: Edit the Deployment to Fix the Image
```
k edit deployment.apps/python-deployment-datacenter
```
- Update the container image to the correct one:
  image: poroko/flask-demo-app

<img width="664" height="505" alt="image" src="https://github.com/user-attachments/assets/0ee4ea52-3146-420c-8eb1-63305ba332c3" />


- Save and exit.
- Kubernetes will automatically start pulling the corrected image and create a new pod.  


## step3: Verify Pod Status
```
kubectl get pods
```
<img width="652" height="47" alt="image" src="https://github.com/user-attachments/assets/bcb2a4f8-c016-4f2d-96bb-b1ab8513d774" />


- The new pod is running successfully.
- The old pod with wrong image shows 0/0 and will eventually terminate.
  
## step4: Fix the Service to Use Correct nodeport and python-flask-default port

```
k describe svc python-dervice-datacenter
```
<img width="482" height="302" alt="image" src="https://github.com/user-attachments/assets/15c91407-ba7a-4aa0-911c-6ba90a2f064b" />

```
kubectl edit svc python-service-datacenter
```

- Ensure the ports section matches the Python Flask app default port:
  ports:
  - protocol: TCP
    port: 5000
    targetPort: 5000
    nodePort: 32345

- Save and exit.

<img width="500" height="310" alt="image" src="https://github.com/user-attachments/assets/31ffcdaf-e1b8-4ad6-b10a-ac009e51393e" />


- The Python app pod is now running successfully.
- Service is configured to expose the app on NodePort 32345, and the app is accessible externally via this port.
  ```
  curl http://10.22.0.10:500
  ```

<img width="540" height="48" alt="image" src="https://github.com/user-attachments/assets/82be14d8-bbc9-4cac-9fa8-0f0ee6ee99a0" />

