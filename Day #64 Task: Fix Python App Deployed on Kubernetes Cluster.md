# Day #64 task: Fix Python App Deployed on Kubernetes Cluster






## step1: Check the Current Deployment and Pod Status
```
k get all
```
```
kubectl get all | grep python-deployment-xfusion
```

- observe the issue and solve the problem , by describing pod
  ```
  k describe pod <pod-name>
  ```

**Findings:**
The image name in the deployment is incorrect: poroko/flask-app-demo
Correct public image on Docker Hub: poroko/flask-demo-app

## step2: Edit the Deployment to Fix the Image
```
k edit deployment <deployment-name>
```
- Update the container image to the correct one:
  image: poroko/flask-demo-app

- Save and exit.
- Kubernetes will automatically start pulling the corrected image and create a new pod.  


## step3: Verify Pod Status
```
kubectl get all | grep python-deployment-xfusion
```


- The new pod is running successfully.
- The old pod with wrong image shows 0/0 and will eventually terminate.
  
## step4: Fix the Service to Use Correct NodePort
```
kubectl edit svc python-service-xfusion
```

- Ensure the ports section matches the Python Flask app default port:
  ports:
  - protocol: TCP
    port: 5000
    targetPort: 5000
    nodePort: 32345

- Save and exit.



- The Python app pod is now running successfully.
- Service is configured to expose the app on NodePort 32345, and the app is accessible externally via this port.
