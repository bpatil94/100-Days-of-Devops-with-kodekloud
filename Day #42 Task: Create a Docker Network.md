# Day #42 task: Create a Docker Network
The Nautilus DevOps team needs to set up several docker environments for different applications. One of the team members has been assigned a ticket where he has been asked to create some docker networks to be used later. Complete the task based on the following ticket description:


a. Create a docker network named as ecommerce on App Server 3 in Stratos DC.


b. Configure it to use bridge drivers.


c. Set it to use subnet 192.168.30.0/24 and iprange 192.168.30.0/24.


- Infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details


## step 1: ssh into the app server
```
ssh banner@stapp03
```
<img width="694" height="135" alt="image" src="https://github.com/user-attachments/assets/7777e258-a6df-4ec4-a3c0-d2126a5bb2ad" />

## step 2 : Verify the named Network already existed or not
```
docker network ls
```
<img width="385" height="189" alt="image" src="https://github.com/user-attachments/assets/82aefef6-db80-46b5-a105-3311fe1d1158" />

## step 3: create docker network
```
docker network create --subnet 192.168.30.0/24 --ip-range 192.168.30.0/24 -d bridge ecommerce
```
<img width="1026" height="42" alt="image" src="https://github.com/user-attachments/assets/35c6c26f-faa1-45cb-ba8b-a997e4677977" />

## step 4 : Verify and inspect the network created
```
docker network ls
```
```
docker network inspect ecommerce
```

<img width="730" height="481" alt="image" src="https://github.com/user-attachments/assets/d42e7b64-6dcb-475c-b739-81bacfbfe153" />

