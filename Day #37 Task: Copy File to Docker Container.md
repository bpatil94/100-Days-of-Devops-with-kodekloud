# Day #37 task: Copy File to Docker Container

The Nautilus DevOps team possesses confidential data on App Server 2 in the Stratos Datacenter. A container named ubuntu_latest is running on the same server.



Copy an encrypted file /tmp/nautilus.txt.gpg from the docker host to the ubuntu_latest container located at /home/. Ensure the file is not modified during this operation.


- Infrastructure Details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details

## 1. SSH into App Server 2
```
ssh steve@stappo2
```
<img width="784" height="178" alt="image" src="https://github.com/user-attachments/assets/07e5b7ec-b1b4-4e0e-b131-162a93aff614" />

- Then add the user to the Docker group so you can run Docker commands without sudo:
```
sudo usermod -aG docker $USER
```

## 2. Verify the running container
```
docker ps
```
<img width="848" height="63" alt="image" src="https://github.com/user-attachments/assets/cdd3278d-dd17-4c9d-b28c-6a072ff7b89a" />

## 3. Locate the file to be copied

```
ls -l /tmp/
```
<img width="981" height="199" alt="image" src="https://github.com/user-attachments/assets/8700daf9-96c2-4ba5-a949-9a4a133a083f" />


## 4. Copy the file into the container
```
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/home/
```
- docker cp — copies files between the Docker host and a container
- /tmp/nautilus.txt.gpg — source path on the Docker host
- ubuntu_latest:/home/ — destination path inside the container
  
<img width="643" height="38" alt="image" src="https://github.com/user-attachments/assets/2c032503-8467-4e0c-8111-85dccd8ff4e1" />

## 5. Confirm the file inside the container
```
docker exec -it ubuntu_latest ls -l /home/
```
<img width="540" height="76" alt="image" src="https://github.com/user-attachments/assets/449a4512-4097-4d87-b3e8-385b338030a2" />
