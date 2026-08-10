# Day 45: Resolve Dockerfile Issues
The Nautilus DevOps team is working to create new images per requirements shared by the development team. One of the team members is working to create a Dockerfile on App Server 1 in Stratos DC. While working on it she ran into issues in which the docker build is failing and displaying errors. Look into the issue and fix it to build an image as per details mentioned below:

a. The Dockerfile is placed on App Server 1 under /opt/docker directory.

b. Fix the issues with this file and make sure it is able to build the image.

c. Do not change base image, any other valid configuration within Dockerfile, or any of the data been used — for example, index.html.

Note: Please note that once you click on FINISH button all the existing containers will be destroyed and new image will be built from your Dockerfile.

- Infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details

# step 1: ssh into the app server
```
ssh tony@stapp01
```
<img width="779" height="135" alt="image" src="https://github.com/user-attachments/assets/f393d500-9168-4ea3-986f-08c0138dc0f8" />

# step 2 : cd to docker file path (/opt/docker)
```
cd /opt/docker/
```
<img width="773" height="211" alt="image" src="https://github.com/user-attachments/assets/b0f3afaf-2d5a-45b7-a68e-c663bff81a93" />

- see the Docker file content
  ```
  cat Dockerfile
  ```
  <img width="832" height="254" alt="image" src="https://github.com/user-attachments/assets/85b42929-c763-48b9-a477-5a61e10111fb" />


# step 3 : just run build command
```
docker build .
```
<img width="1243" height="439" alt="image" src="https://github.com/user-attachments/assets/922901a2-4691-4373-b597-48a2129ce8ff" />

 <img width="1240" height="387" alt="image" src="https://github.com/user-attachments/assets/337e2011-10db-4e6f-bdd0-5f49ae8cd706" />



- it will through the error , go with error first

- in my case path of certs were wrong so i added correct path

<img width="868" height="323" alt="image" src="https://github.com/user-attachments/assets/57cc9f3c-507e-4ee0-a338-9ae20388d796" />

**Note**
The correct approach was to use COPY instead of RUN cp.

✅ COPY transfers files from the local build context into the image.
❌ RUN cp only works on files that already exist inside the container.


# step 4 : again build command
```
docker build -t my-apache .
```
<img width="998" height="361" alt="image" src="https://github.com/user-attachments/assets/9dd4fb2a-ed70-43b5-8036-8fa7bf9d112f" />

<img width="1173" height="343" alt="image" src="https://github.com/user-attachments/assets/1dfc5485-ddb8-4ec7-b4f7-a253b411eb4f" />


# step 5 :see the images
```
docker images
```
<img width="834" height="64" alt="image" src="https://github.com/user-attachments/assets/3d5c1faf-127a-4ac9-a5fb-364204c2ae85" />

