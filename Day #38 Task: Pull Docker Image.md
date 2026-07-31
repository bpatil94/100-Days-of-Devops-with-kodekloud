# Day #38 task: Pull Docker Image
Nautilus project developers are planning to start testing on a new project. As per their meeting with the DevOps team, they want to test containerized environment application features. As per details shared with DevOps team, we need to accomplish the following task:


a. Pull busybox:musl image on App Server 1 in Stratos DC and re-tag (create new tag) this image as busybox:blog.


- Infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details

## 1. SSH into App Server 1
```
ssh tony@stapp01
```
<img width="680" height="321" alt="image" src="https://github.com/user-attachments/assets/3d426af6-f976-4f0f-8487-b1b2c154c8e1" />

- or use this, add the user to the Docker group so you can run Docker commands without sudo:
  ```
  sudo usermod -aG docker $USER
  ```
## 2. Pull the specified image
```
  docker pull busybox:musl
```
<img width="725" height="210" alt="image" src="https://github.com/user-attachments/assets/f9752d1e-5c3e-46ac-956d-457691b57492" />

## 3. Re-tag the image
- Create a new tag named blog using the docker tag command:
  
```
docker tag busybox:musl busybox:news
```
<img width="726" height="21" alt="image" src="https://github.com/user-attachments/assets/9802586b-5daa-4274-af15-d7839bbfa67c" />

- This doesn’t duplicate the image; it simply creates a new label that references the same image ID.
  
## 4. Verify the tag
- List all images to confirm both tags now exist:
  
```
docker images
```
<img width="851" height="119" alt="image" src="https://github.com/user-attachments/assets/a6300f6b-4c89-46dd-b286-0a5a3fc494dc" />


- Notice that both musl and blog share the same IMAGE ID, which confirms that the retagging worked correctly.
