# Day 54: Kubernetes Shared Volumes
We are working on an application that will be deployed on multiple containers within a pod on Kubernetes cluster. There is a requirement to share a volume among the containers to save some temporary data. The Nautilus DevOps team is developing a similar template to replicate the scenario. Below you can find more details about it.

1. Create a pod named volume-share-devops.

2. For the first container, use image fdebian with latest tag only and remember to mention the tag i.e debian:latest, container should be named as volume-container-devops-1, and run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/official.

3. For the second container, use image debian with the latest tag only and remember to mention the tag i.e debian:latest, container should be named as volume-container-devops-2, and again run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/apps.

4. Volume name should be volume-share of type emptyDir.

5. After creating the pod, exec into the first container i.e volume-container-devops-1, and just for testing create a file media.txt with the content Welcome to xFusionCorp Industries under the mounted path of first container i.e /tmp/official.
6. 
7. The file media.txt should be present under the mounted path /tmp/apps on the second container volume-container-devops-2 as well, since they are using a shared volume.

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.



**The Mission: Sharing Temporary Data**
The objective is straightforward:

1. Create a Pod named volume-share-xfusion.
2. Deploy two containers within this Pod, both using the debian:latest image.
3. Define a shared volume named volume-share using the emptyDir type.
4. Mount this shared volume to different paths in each container: /tmp/beta in the first and /tmp/apps in the second.
5. Verify that data written from one container is immediately accessible in the other.



**Important points about emptyDir**
--> The volume is created when the Pod starts.
--> All containers inside the Pod can share it.
--> Data remains available if an individual container restarts.
--> Data is deleted when the Pod is removed.
--> It is commonly used for temporary/shared data between containers.



## step1:create the simple pod manifest file
```
kubectl version
```
```
ls
```
<img width="631" height="132" alt="image" src="https://github.com/user-attachments/assets/fe081ad3-5d39-4d51-bd33-e73ab61c0ac4" />

```
vi pod.yaml
```
- insert the content
```
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-devops
spec:
  volumes:
  - name: volume-share
    emptyDir: {}
  containers:
  - name: volume-container-devops-1
    image: debian:latest
    volumeMounts:
    - name: volume-share
      mountPath: /tmp/official
    command: ["sleep","3600"]
  - name: volume-container-devops-2
    image: debian:latest
    volumeMounts:
    - name: volume-share
      mountPath: /tmp/apps
    command: ["sleep","3600"]
```
<img width="388" height="376" alt="image" src="https://github.com/user-attachments/assets/9179c690-1c71-4b28-a5ec-a4151bc140b6" />

## step2: Deploy the pod
```
kubectl apply -f pod.yaml
```
<img width="591" height="273" alt="image" src="https://github.com/user-attachments/assets/d8dee007-36d8-489b-99d2-41e062150d23" />

## step3: Check the pod status and see containers
```
kubectl get pods
```
```
kubectl get pods -o jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.containers[*]}{.name}{" ("}{.image}{") "}{end}{end}'
```
<img width="1163" height="51" alt="image" src="https://github.com/user-attachments/assets/28a0b34c-e763-4518-a9f8-6d2352aed78a" />

## step4: Exec into one of the container and and create one sample file in mentioned path
```
kubectl exec -it volume-share-devops -c volume-container-devops-1 -- bash
```
```
pwd
```
```
cd /tmp/official/
```
```
echo "Welcome to xFusionCorp Industries" > official.txt
```

## step5: List, cat the content of file and exit the container
```
ls
```
```
cat official.txt
```
```
exit
```
<img width="729" height="270" alt="image" src="https://github.com/user-attachments/assets/5a916e6e-8437-469b-ad44-47f2d8f1dc5f" />


## step6: Exec into the volume-container-devops-2 container of the volume-share-devops pod
```
kubectl exec -it volume-share-devops -c volume-container-devops-2 -- bash
```

## step7: Verify if you can see the file and it's content in the mounted path
```
cd /tmp/apps/
```
```
ls
```
```
cat official.txt
```
```
exit 
```   
<img width="781" height="255" alt="image" src="https://github.com/user-attachments/assets/687ae126-70fd-48c5-be7a-47fb6bddc1c7" />

## step8: We can also validate it, without having to actually go inside the container.
- For container1
```
kubectl exec -it volume-share-devops -c volume-container-devops-1 -- ls /tmp/official/ 
```
```
kubectl exec -it volume-share-devops -c volume-container-devops-1 -- cat /tmp/official/official.txt
```

<img width="878" height="97" alt="image" src="https://github.com/user-attachments/assets/5ae8f88a-dd70-44d7-9fe7-796353f3bc3a" />

 - For container2
```
kubectl exec -it volume-share-devops -c volume-container-devops-2 -- ls  /tmp/apps/
```

```
kubectl exec -it volume-share-devops -c volume-container-devops-2 -- cat /tmp/apps/official.txt
```
<img width="868" height="146" alt="image" src="https://github.com/user-attachments/assets/fdcb3cef-f830-4d76-9196-3a653d5b5759" />

Note: As seen, whatever files are created in the mounted path in any container of the same pod, we should be able to access the same file in the mounted path of the other containers too!


**Some examples of such scenarios are as follows:**

1. Web + App Containers: Nginx serves static files while PHP writes dynamic content to the same volume, e.g., /var/www/html, no network copy needed. Similar to how we did in Day 53.
2. Temporary Storage: Container-1 writes intermediate data to shared volume and container-2 reads, processes, and uploads it.
3. Persistent State: Persistent Volume shares data across pods, e.g., multiple WordPress replicas serving the same /var/www/html.










