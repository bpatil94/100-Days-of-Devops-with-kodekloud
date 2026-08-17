# Day #53 task: Resolve VolumeMounts Issue in Kubernetes

We encountered an issue with our Nginx and PHP-FPM setup on the Kubernetes cluster this morning, which halted its functionality. Investigate and rectify the issue:



The pod name is nginx-phpfpm and configmap name is nginx-config. Identify and fix the problem.


Once resolved, copy /home/thor/index.php file from the jump host to the nginx-container within the nginx document root. After this, you should be able to access the website using Website button on the top bar.


Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.



## step 1: Check existing running pods and deployment details
```
kubectl get all
```
or
```
kubectl get pods
```
<img width="666" height="170" alt="image" src="https://github.com/user-attachments/assets/3027d37c-0f1b-4b47-a76c-506a5626db87" />


## step 2: Check the shared volume path in the existing config map
```
kubectl get configmap
```
```
kubectl describe configmap nginx-config
```
<img width="590" height="470" alt="image" src="https://github.com/user-attachments/assets/748b5d5d-e170-4dc6-bdb3-abf83de7c8ea" />


## step 3: Get the pod yaml file from the running pod, save to tmp and review:
```
kubectl get pod nginx-phpfpm -o yaml > /tmp/nginx.yaml
```
```
cat /tmp/nginx.yaml
```
<img width="849" height="433" alt="image" src="https://github.com/user-attachments/assets/581481d6-6ebc-4237-afef-8cb7993a77aa" />
<img width="631" height="449" alt="image" src="https://github.com/user-attachments/assets/9a1f58f4-e62c-442c-a810-dbb4f33775ce" />


- Identify that the PHP container is mounting /usr/share/nginx/html instead of /var/www/html. Update the pod YAML to fix the volume mount:
- Make revelant changes and then save file 
  ```
  sed -i 's|/usr/share/nginx/html|/var/www/html|g' /tmp/nginx.yaml
   ```
<img width="714" height="21" alt="image" src="https://github.com/user-attachments/assets/d32cd435-4b70-4b15-8e96-3f0bb1196b66" />


## step 4: Post changes using force

Replace the pod with the corrected configuration:
```
kubectl replace -f /tmp/nginx.yaml --force

<img width="467" height="47" alt="image" src="https://github.com/user-attachments/assets/5717f007-9874-4a1c-8138-224ad903e248" />

## step 5: check the running status of pods
```
kubectl get pods
```
<img width="457" height="49" alt="image" src="https://github.com/user-attachments/assets/874a0e41-d45f-4ef0-9b35-8ab38d527e87" />


## step 6: Copy the index.php from jump host to the nginx-container
```
kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html -c nginx-container

```
<img width="750" height="47" alt="image" src="https://github.com/user-attachments/assets/831869d3-3001-4624-a198-c91621a89c4d" />

## step 7: validate task by clicking webside on right top corner, you will see the php page

<img width="1365" height="276" alt="image" src="https://github.com/user-attachments/assets/5b2f9709-b652-4bf3-ac6b-169bb9ab3d41" />

<img width="1356" height="715" alt="image" src="https://github.com/user-attachments/assets/ab235e66-0a25-4d7f-9dde-9f04d5c0ad9d" />







