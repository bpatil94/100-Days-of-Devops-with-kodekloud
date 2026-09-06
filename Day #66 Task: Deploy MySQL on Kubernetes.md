# Day #66 task: Deploy MySQL on Kubernetes
A new MySQL server needs to be deployed on Kubernetes cluster. The Nautilus DevOps team was working on to gather the requirements. Recently they were able to finalize the requirements and shared them with the team members to start working on it. Below you can find the details:

1.) Create a PersistentVolume mysql-pv, its capacity should be 250Mi, set other parameters as per your preference.

2.) Create a PersistentVolumeClaim to request this PersistentVolume storage. Name it as mysql-pv-claim and request a 250Mi of storage. Set other parameters as per your preference.

3.) Create a deployment named mysql-deployment, use any mysql image as per your preference. Mount the PersistentVolume at mount path /var/lib/mysql.

4.) Create a NodePort type service named mysql and set nodePort to 30007.

5.) Create a secret named mysql-root-pass having a key pair value, where key is password and its value is YUIidhb667, create another secret named mysql-user-pass having some key pair values, where first key is username and its value is kodekloud_cap, second key is password and value is dCV3szSGNA, create one more secret named mysql-db-url, key name is database and value is kodekloud_db4

6.) Define some environment variables within the container:
  a.) name: MYSQL_ROOT_PASSWORD, should pick value from secretKeyRef name: mysql-root-pass and key: password
  b.) name: MYSQL_DATABASE, should pick value from secretKeyRef name: mysql-db-url and key: database
  c.) name: MYSQL_USER, should pick value from secretKeyRef name: mysql-user-pass key key: username
  d.) name: MYSQL_PASSWORD, should pick value from secretKeyRef name: mysql-user-pass and key: password


Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.


# step1:  Create the PV manifest with the given details and create the PV using the manifest.

This problem touches on all the following things that we’ve practiced and learned in kubernetes till now:
- Persistent Volume. (PV)
- Persistent Volume claim. (PVC)
- Secrets.
- Deployment with environment variables from secret.
- Services.
Let’s get started with this.

```
vi pv.yaml
```
- add the contents
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 250Mi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/dba"
```
<img width="251" height="253" alt="image" src="https://github.com/user-attachments/assets/43f5ff92-fdb9-4448-968e-3961f19dc51f" />

```
k apply -f pv.yaml
```
```
k get pv
```
<img width="905" height="138" alt="image" src="https://github.com/user-attachments/assets/22c6c835-2c1d-4f93-80e1-ba772910f5c5" />


# Step-2: Create the PVC manifest and create the PVC using the manifest.
```
vi pvc.yaml
```
- add the contents

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 250Mi
```
<img width="309" height="208" alt="image" src="https://github.com/user-attachments/assets/eda49556-4b84-4280-8e4f-81a446adbdbb" />

```
k apply -f pvc.yaml
```
```
k get pvc
```
```
k get pv
```
<img width="1042" height="119" alt="image" src="https://github.com/user-attachments/assets/d7186640-9a90-432c-ab43-29bc464fa5b0" />

# Step-3: Create the secrets using the details given in the problem statement.
```
kubectl create secret generic mysql-user-pass --from-literal='username=kodekloud_cap' --from-literal='password=dCV3szSGNA'
```
```
kubectl create secret generic mysql-root-pass --from-literal='password=YUIidhb667'
```
```
kubectl create secret generic mysql-db-url --from-literal='database=kodekloud_db4'
```
```
kubectl get secrets
```
<img width="1028" height="194" alt="image" src="https://github.com/user-attachments/assets/bdc7671f-3c5d-448c-beb9-8625ee1c92ae" />



# Step-4: Create the deployment manifest with the details given and create the deployment using the manifest.

```
vi deployment.yaml
```
- add the contents

```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: mysql-deployment
  name: mysql-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql-deployment
  strategy: {}
  template:
    metadata:
      labels:
        app: mysql-deployment
    spec:
      volumes:
        - name: mysql-pvc
          persistentVolumeClaim:
           claimName: mysql-pv-claim
      containers:
      - image: mysql:8.0
        name: mysql-container
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-root-pass
              key: password
        - name: MYSQL_DATABASE  
          valueFrom:
            secretKeyRef:
              name: mysql-db-url
              key: database
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: username
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: password
        volumeMounts:
        - name: mysql-pvc
          mountPath: /var/lib/mysql

```
<img width="347" height="499" alt="image" src="https://github.com/user-attachments/assets/99441d01-a58d-49d1-bbae-3f81cba4c6dc" />

<img width="546" height="400" alt="image" src="https://github.com/user-attachments/assets/4eba01ae-3459-436a-bbe5-57aea26af1fd" /> 


```
kubectl apply -f deployment.yaml
```
```
kubectl get deploy
```
<img width="562" height="106" alt="image" src="https://github.com/user-attachments/assets/d3a6ac68-cef7-43a9-8d2f-795b34fc65a6" />

```
kubectl describe deploy
```
<img width="819" height="372" alt="image" src="https://github.com/user-attachments/assets/736a0c17-1913-403e-9f08-57dca275564f" />

<img width="868" height="405" alt="image" src="https://github.com/user-attachments/assets/f51f4579-dd0d-417a-b2c9-b8158d1ffefd" />


# Step-5: Create the service manifest with the details given and create the service using the manifest.

```
vi svc.yaml
```
- add the content

```
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  type: NodePort
  selector:
    app: mysql-deployment
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30007
```
<img width="287" height="215" alt="image" src="https://github.com/user-attachments/assets/68fb6b70-85b6-4d43-a1f6-296e3a9c211f" />

```
kubectl apply -f svc.yaml
```
```
k get svc
```

```
kubectl describe svc mysql
```
<img width="728" height="421" alt="image" src="https://github.com/user-attachments/assets/2e0235d5-ecdf-445b-9a40-6332d8159ef1" />

# Step-6: Validate if everything is setup as expected in the problem statement.

```
kubectl exec -it mysql-deployment-86bd6fcfdc-t7bvn -- env | grep MYSQL
```
<img width="723" height="178" alt="image" src="https://github.com/user-attachments/assets/e34bd9bc-f345-40d7-92ac-80910f03902b" />

### # Check MySQL is Actually Running. We can see that by looking at the pod logs.

```
kubectl logs mysql-deployment-86bd6fcfdc-t7bvn
```
<img width="1222" height="434" alt="image" src="https://github.com/user-attachments/assets/d6e23c15-6259-46de-a91f-f056fced36db" />

<img width="1237" height="467" alt="image" src="https://github.com/user-attachments/assets/60560f2d-b2be-47c8-90b4-eb2aa32cec32" />


### # Test MySQL Login as a root user and enter the password given in the problem statement and see if you can successfully login as the root user.
```
kubectl exec -it mysql-deployment-86bd6fcfdc-t7bvn -- mysql -u root -p
```
- give password for root given in description, you will be able to log in to mysql
- 
<img width="853" height="256" alt="image" src="https://github.com/user-attachments/assets/967f6056-6b92-4123-995b-1180503935e8" />

### # Test MySQL Login as a kodekloud_tim user and enter the password given in the problem statement and see if you can successfully login as the kodekloud_tim user.
```
kubectl exec -it mysql-deployment-86bd6fcfdc-t7bvn -- mysql -u kodekloud_cap -p
```
- give the user password given in the description and you will be in mysql

<img width="826" height="262" alt="image" src="https://github.com/user-attachments/assets/5e3cd96d-43dc-4af1-8591-1a32b78f226e" />


