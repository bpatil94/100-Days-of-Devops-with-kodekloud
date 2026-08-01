# Day #39 task: Create a Docker Image From Container

One of the Nautilus developer was working to test new changes on a container. He wants to keep a backup of his changes to the container. A new request has been raised for the DevOps team to create a new image from this container. Below are more details about it:


a. Create an image apps:nautilus on Application Server 1 from a container ubuntu_latest that is running on same server.

- Infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details

## 1. SSH into App Server 1
```
ssh tony@stapp01
```
- Then add the user to the Docker group so you can run Docker commands without sudo:
  ```
  sudo usermod -aG docker $USER
  ```
<img width="680" height="322" alt="image" src="https://github.com/user-attachments/assets/78b8e5b4-dc68-4da8-933f-d6b1ad60f6d9" />

## 2. Check the running container
```
docker ps
```
<img width="818" height="120" alt="image" src="https://github.com/user-attachments/assets/760e9c36-6f65-4d18-9858-9779fdabd2a7" />

Note: If it’s stopped, start it again with:
```
docker start ubuntu_latest
```
## 3. Commit the container as an image
```
docker commit ubuntu_latest apps:nautilus
```
<img width="642" height="42" alt="image" src="https://github.com/user-attachments/assets/a98110f3-c7ec-4b35-aadb-0e45ea5a6e32" />

Command breakdown:
  - docker commit — saves a container’s state as a new image.
  - ubuntu_latest — source container name.
  - apps:nautilus — repository (demo) and tag (nautilus) for the new image.


## 4. Verify the new image
```
docker images
```
<img width="569" height="93" alt="image" src="https://github.com/user-attachments/assets/f1ad8940-6ad7-4b86-85b2-1897f46d4066" />
