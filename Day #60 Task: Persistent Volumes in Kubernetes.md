# Day #60 task: Persistent Volumes in Kubernetes
The Nautilus DevOps team is working on a Kubernetes template to deploy a web application on the cluster. There are some requirements to create/use persistent volumes to store the application code, and the template needs to be designed accordingly. Please find more details below:


1. Create a PersistentVolume named as pv-devops. Configure the spec as storage class should be manual, set capacity to 3Gi, set access mode to ReadWriteOnce, volume type should be hostPath and set path to /mnt/itadmin (this directory is already created, you might not be able to access it directly, so you need not to worry about it).

2. Create a PersistentVolumeClaim named as pvc-devops. Configure the spec as storage class should be manual, request 1Gi of the storage, set access mode to ReadWriteOnce.

3. Create a pod named as pod-devops, mount the persistent volume you created with claim name pvc-devops at document root of the web server, the container within the pod should be named as container-devops using image nginx with latest tag only (remember to mention the tag i.e nginx:latest).

4. Create a node port type service named web-devops using node port 30008 to expose the web server running within the pod.

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.




***Note***
What is a persistent volume (PV)?
In Kubernetes, a Persistent Volume is storage in the cluster that exists independently of Pods.
Even if a Pod is deleted or restarted, the data stored in the PV remains safe. You can think of it like a hard drive attached to the cluster.

What is a persistent volume claim (PVC)?
A Persistent Volume Claim is a request made by a Pod for storage.
The Pod asks Kubernetes for storage (for example 5 GB), and Kubernetes connects that request to a matching PV.

A simple analogy that will help you understand what PV and PVC are. You can think of:
- PV as the actual storage disk.
- PVC as the request to use that disk and how much of that disk you want to use.
- Pod as the application that uses the storage.

So the flow is:
Pod → PVC (request) → PV (actual storage).

=================================== ================================================= ========================================== 

## Step-1: Create a persistent volume through the manifest file.
Let’s create the manifest file (pv.yaml) using the details given to us in the problem statement. The details are:

- Name of the PV: pv-datacenter
- Storage class name: manual
- Capacity of the PV: 5Gi
- Access Mode: ReadWriteOnce
- Volume Type: hostPath
- Path: /mnt/dba
Host Path means the volume will use a directory from the Kubernetes node (host machine). The path /mnt/dba is the directory on the node's filesystem that will be used for storage. This directory will then be mounted inside the container so the application can read/write files there.

```
vi pv.yaml
```
- insert the content

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-devops
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 3Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/itadmin"
```
<img width="308" height="233" alt="image" src="https://github.com/user-attachments/assets/bf0ed128-485c-4935-83d2-93c53fdf7786" />

```
kubectl apply -f pv.yaml
```
```
kubectl get pv
```
<img width="945" height="117" alt="image" src="https://github.com/user-attachments/assets/47f33e3b-3500-432b-a0f5-237c7d9e172e" />

## Step-2: Create a persistent volume claim through the manifest file.
Let’s create the manifest file (pvc.yaml) using the details given to us in the problem statement. The details are:

- Name of the PVC: pvc-datacenter
- Storage class name: manual
- Storage Request from PV: 3Gi
- Access Mode: ReadWriteOnce

```
vi pvc.yaml
```
- insert the content

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-devops
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```
<img width="296" height="198" alt="image" src="https://github.com/user-attachments/assets/77c11cdf-3c31-4804-80c9-933f8709c71d" />

```
kubectl apply -f pvc.yaml
```
```
kubectl get pvc
```
```
kubectl get pv
```
<img width="1020" height="187" alt="image" src="https://github.com/user-attachments/assets/4ace4416-e843-4f5e-aaaf-f519c6eb84a0" />


***What does it mean, when the PV is bound?***
When a PersistentVolume (PV) is Bound, it means the PV has been successfully connected to a PersistentVolumeClaim (PVC).In other words, the storage request (PVC) found a matching storage disk (PV), and now that storage is reserved for that claim.

Another question that will pop up, is that, how does the PVC know which PV to use, as we are not really specifying any PV details in the manifest, right?

A PersistentVolumeClaim (PVC) does not explicitly specify which PersistentVolume (PV) to use. Instead, Kubernetes automatically binds the PVC to a matching PV based on criteria such as storage size, access modes, and storageClassName.

In our case, the PVC requested 3Gi of storage, so Kubernetes searched for a PV that could provide at least 3Gi and found pv-datacenter, which satisfied the requirements. Therefore, the PVC was bound to that PV.


## Step-3: Create a pod through the manifest file.
Let’s create the manifest file (pod.yaml) using the details given to us in the problem statement. The details are:

- Name of the pod: pod-datacenter
- PV to mount: pv-datacenter
- PVC to declare: pvc-datacenter
- Image: httpd:latest
- Container-name: container-datacenter

```
vi pod.yaml
```
- insert the content

```
apiVersion: v1
kind: Pod
metadata:
  name: pod-devops
# Also make sure, you are adding a label, so that we can tell the service to expose this pod. More like an identifier for the service.
  labels:
    purpose: pod-devops
spec:
  volumes:
    - name: pv-devops
      persistentVolumeClaim:
        claimName: pvc-devops
  containers:
    - name: container-devops
      image: nginx:latest
      ports:
        - containerPort: 80
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: pv-devops
```
<img width="683" height="373" alt="image" src="https://github.com/user-attachments/assets/ff8ddd2b-585d-4220-985d-580df2bf2fd3" />

```
kubectl apply -f pod.yaml
```
```
kubectl describe pod
```

<img width="873" height="449" alt="image" src="https://github.com/user-attachments/assets/d95695ca-ef2c-4ba9-a0c7-48d61c91a3b9" />

<img width="843" height="411" alt="image" src="https://github.com/user-attachments/assets/c064378e-de69-4c1f-8bfe-95968520773c" />

<img width="1122" height="371" alt="image" src="https://github.com/user-attachments/assets/24f53c6f-756a-47ac-8b3e-e4e3ab1fe619" />


## Step-4: Create a service exposing the created pod through the manifest file.
Let’s create the manifest file (svc.yaml) using the details given to us in the problem statement. The details are:

- Name of the service: web-datacenter
- Service Type: Node Port.
- Node Port: 30008

```
vi service.yaml
```

- insert the content

```
apiVersion: v1
kind: Service
metadata:
  name: web-datacenter
spec:
  type: NodePort
  selector:
# Ensure you add the same label that you've used to create the pod.
    purpose: pod-datacenter
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30008`
```
<img width="541" height="206" alt="image" src="https://github.com/user-attachments/assets/5436e849-0922-4129-b714-b47704f28712" />

```
kubectl apply -f service.yaml
```
```
kubectl get service
```

<img width="613" height="121" alt="image" src="https://github.com/user-attachments/assets/749407b1-b077-4b7d-97fd-b0b5ad47df52" />

## validation
Check if you can access the website now. (Click on the website button on the top right corner).

OR

## Verification Commands and Final State
Verification confirmed that all components are Bound and Running.

