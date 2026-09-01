# Day #62 task: Manage Secrets in Kubernetes
The Nautilus DevOps team is working to deploy some tools in Kubernetes cluster. Some of the tools are licence based so that licence information needs to be stored securely within Kubernetes cluster. Therefore, the team wants to utilize Kubernetes secrets to store those secrets. Below you can find more details about the requirements:

1. We already have a secret key file beta.txt under the /opt/ directory. Create a generic secret named beta, it should contain the password/license-number present in beta.txt file.

2. Also create a pod named secret-nautilus.

3. Configure pod's spec as container name should be secret-container-nautilus, image should be debian with latest tag (remember to mention the tag with image). Use sleep command for container so that it remains in running state. Consume the created secret and mount it under /opt/apps within the container.

4. To verify you can exec into the container secret-container-nautilus, to check the secret key under the mounted path /opt/apps. Before hitting the Check button please make sure pod/pods are in running state, also validation can take some time to complete so keep patience.

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.



## Step-1: Explore the content of the file and create a generic secret using the same file.
```
cat /opt/beta.txt
```
<img width="526" height="180" alt="image" src="https://github.com/user-attachments/assets/c4fe027f-4c59-4896-924b-03f06519104a" />

<img width="594" height="113" alt="image" src="https://github.com/user-attachments/assets/35ffd590-c9e5-46e3-b60d-631662e2b25e" />


```
kubectl create secret generic beta --from-file=/opt/beta.txt
```
- kubectl create secret generic → Creates an opaque (generic) Secret.
- official → Name of the Kubernetes Secret.
- --from-file=/opt/beta.txt → Uses the file /opt/official.txt to create the Secret.

```
kubectl get secret
```
```
kubectl describe secret beta
```
<img width="578" height="292" alt="image" src="https://github.com/user-attachments/assets/b542a9e2-9696-4761-8f0e-6d10171beed1" />

<img width="398" height="269" alt="image" src="https://github.com/user-attachments/assets/4b7ea57e-993e-4a1e-b49c-08e4be5677ef" />


## Step-2: Create a manifest file, using the given specs and create a pod using it.

Let’s first jot down the specs given in the problem statement.
- Pod Name: secret-nautilus.
- Container Name:secret-container-nautilus.
- Image: debian:latest
- Mounted Path: /opt/apps.
- Command: ['sleep’,'6000'].

```
vi pod.yaml
```
```
apiVersion: v1
kind: Pod
metadata:
  name: secret-nautilus
spec:
  volumes:
    - name: secret-volume
      secret:
        secretName: beta
  containers:
    - name: secret-container-nautilus
      image: debian:latest
      command: ['sleep','6000']
      volumeMounts:
        - name: secret-volume
          readOnly: true
          mountPath: "/opt/apps"
```
<img width="398" height="269" alt="image" src="https://github.com/user-attachments/assets/057b09bb-c472-4451-97e5-045357df84eb" />

```
kubectl apply -f pod.yaml
```
<img width="546" height="79" alt="image" src="https://github.com/user-attachments/assets/75da5349-b901-4411-b7b9-69d2c2541ae5" />

## Step-3: Validate if the pod is running and validate if the secret is mounted successfully within the container.

```
kubectl get po
```
<img width="459" height="47" alt="image" src="https://github.com/user-attachments/assets/37eae999-aeb1-42f8-8158-a8813ae59ece" />


```
kubectl describe pod
```
<img width="903" height="511" alt="image" src="https://github.com/user-attachments/assets/47ec7128-baf6-4954-bdc5-10381ee76da9" />

<img width="1145" height="486" alt="image" src="https://github.com/user-attachments/assets/2ba0f645-7c50-49e7-b04a-15f1c2085ce9" />


## see the things inside the container
```
kubectl exec -it secret-nautilus -c secret-container-nautilus -- ls -l /opt/apps/
```
<img width="753" height="51" alt="image" src="https://github.com/user-attachments/assets/332fd9bf-02d2-4669-bf36-111bf06c4d1e" />

```
kubectl exec -it secret-nautilus -c secret-container-nautilus -- cat /opt/apps/beta.txt
```
<img width="742" height="38" alt="image" src="https://github.com/user-attachments/assets/c4396b0b-d9f5-4ab4-b8e1-c414a57fea80" />

