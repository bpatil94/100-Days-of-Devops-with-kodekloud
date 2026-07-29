# Day #36 task: Deploy Nginx Container on Application Server

The Nautilus DevOps team is conducting application deployment tests on selected application servers. They require a nginx container deployment on Application Server 1. Complete the task with the following instructions:


On Application Server 1 create a container named nginx_1 using the nginx image with the alpine tag. Ensure container is in a running state.



- Infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details

1️⃣ Connected to the target server
```
ssh tony@stapp01
```
<img width="684" height="137" alt="image" src="https://github.com/user-attachments/assets/9e0fa119-10d7-4601-bc6a-3cdc30357f41" /> 

2️⃣ Pulled the Nginx image with Alpine tag
```
docker pull nginx:alpine
```
 <img width="680" height="327" alt="image" src="https://github.com/user-attachments/assets/967f9ac8-59e0-4223-b785-426978585fa1" />


3️⃣ Created and started the container
```
docker run -d - name nginx_2 nginx:alpine
```
<img width="836" height="44" alt="image" src="https://github.com/user-attachments/assets/a6231e6a-bda1-4881-979c-58cd44f6aa1a" />
 
4️⃣ Verified the deployment
```
docker ps
```
<img width="885" height="84" alt="image" src="https://github.com/user-attachments/assets/cc513fe4-c628-4661-8c49-8f80b8f381ec" />



