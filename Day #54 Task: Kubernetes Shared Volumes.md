# Day 54: Kubernetes Shared Volumes




**The Mission: Sharing Temporary Data**
The objective is straightforward:

1. Create a Pod named volume-share-xfusion.
2. Deploy two containers within this Pod, both using the fedora:latest image.
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

## step2: Deploy the pod
```
kubectl apply -f pod.yaml
```

## step3: Check the pod status
```
kubectl get pods
```
## step4: Exec into one of the container and and create one sample file in mentioned path
```
kubectl exec -it volume-share-devops -c volume-container-devops-1 --bash
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

## step8: We can also validate it, without having to actually go inside the container.
- For container1
```
kubectl exec -it volume-share-devops -c volume-container-devops-1 -- ls /tmp/official/ 
```
```
kubectl exec -it volume-share-devops -c volume-container-devops-1 -- cat /tmp/official/official.txt
```
 - For container2
```
kubectl exec -it volume-share-devops -c volume-container-devops-2 -- ls  /tmp/apps/
```

```
kubectl exec -it volume-share-devops -c volume-container-devops-2 -- cat /tmp/apps/official.txt
```

Note: As seen, whatever files are created in the mounted path in any container of the same pod, we should be able to access the same file in the mounted path of the other containers too!


**Some examples of such scenarios are as follows:**

1. Web + App Containers: Nginx serves static files while PHP writes dynamic content to the same volume, e.g., /var/www/html, no network copy needed. Similar to how we did in Day 53.
2. Temporary Storage: Container-1 writes intermediate data to shared volume and container-2 reads, processes, and uploads it.
3. Persistent State: Persistent Volume shares data across pods, e.g., multiple WordPress replicas serving the same /var/www/html.










