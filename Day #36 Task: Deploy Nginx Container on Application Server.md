# Day #36 task: Deploy Nginx Container on Application Server

The Nautilus DevOps team is conducting application deployment tests on selected application servers. They require a nginx container deployment on Application Server 1. Complete the task with the following instructions:


On Application Server 1 create a container named nginx_1 using the nginx image with the alpine tag. Ensure container is in a running state.



- Infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details

1️⃣ Connected to the target server
```
ssh tony@stapp01
```
<img width="672" height="143" alt="image" src="https://github.com/user-attachments/assets/558551fc-3107-4af2-a23d-f6c3586416f9" />

2️⃣ Pulled the Nginx image with Alpine tag
```
docker pull nginx:alpine
```
<img width="671" height="304" alt="image" src="https://github.com/user-attachments/assets/e3f293f7-d1db-479c-b5d6-8e2641d1df3a" />

3️⃣ Created and started the container
```
docker run -d - name nginx_2 nginx:alpine
```
<img width="755" height="140" alt="image" src="https://github.com/user-attachments/assets/a897824b-8eba-426c-8aa5-e8d3ecc61048" />

4️⃣ Verified the deployment
```
docker ps
```
<img width="942" height="91" alt="image" src="https://github.com/user-attachments/assets/57b89535-5351-42b1-91eb-7e33d20c2cca" />



