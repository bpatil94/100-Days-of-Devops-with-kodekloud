# Day 43: Docker Ports Mapping
The Nautilus DevOps team is planning to host an application on a nginx-based container. There are number of tickets already been created for similar tasks. One of the tickets has been assigned to set up a nginx container on Application Server 3 in Stratos Datacenter. Please perform the task as per details mentioned below:


a. Pull nginx:stable docker image on Application Server 3.


b. Create a container named blog using the image you pulled.


c. Map host port 3000 to container port 80. Please keep the container in running state.

-infrastructure details:  https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details


## step 1: ssh into the app server
```
ssh banner@stapp03
```
<img width="647" height="133" alt="image" src="https://github.com/user-attachments/assets/6a8f950c-20a7-4f87-8735-3c1a6dff4695" />


## step 2 : Verify the named image is already existed or not
```
docker images
```
<img width="543" height="43" alt="image" src="https://github.com/user-attachments/assets/ad86891d-613a-4d0f-8d41-05833a7b2aeb" />

##  step 3 : Pull the nginx:alpine docker image
```
docker pull nginx:stable
```
<img width="694" height="213" alt="image" src="https://github.com/user-attachments/assets/6713575c-8ac6-4d9f-9075-432ef48931ad" />

## step 4 : Create and run a container named 'blog' with port mapping
```
docker run -d --name blog -p 3000:80 nginx:stable
```
<img width="1111" height="94" alt="image" src="https://github.com/user-attachments/assets/236722eb-c0c6-4437-adcf-234a4cc8c060" />

## step 5 : Verify the container is running
```
docker ps
```
<img width="1134" height="95" alt="image" src="https://github.com/user-attachments/assets/e418c5f2-12b3-4779-bfcf-e88b446b8190" />

## step 6 : to see the content of the container on port 3000
```
curl http://localhost:3000
```

<img width="638" height="496" alt="image" src="https://github.com/user-attachments/assets/c3054201-c998-425c-8465-cb18f57455d5" />

