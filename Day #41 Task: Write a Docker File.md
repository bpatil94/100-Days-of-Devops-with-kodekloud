# Day 41: Write a Docker File
As per recent requirements shared by the Nautilus application development team, they need custom images created for one of their projects. Several of the initial testing requirements are already been shared with DevOps team. Therefore, create a docker file /opt/docker/Dockerfile (please keep D capital of Dockerfile) on App server 3 in Stratos DC and configure to build an image with the following requirements:



a. Use ubuntu:24.04 as the base image.


b. Install apache2 and configure it to work on 8083 port. (do not update any other Apache configuration settings like document root etc).

- Infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details

## step 1: ssh into the app server
```
ssh banner@stapp03
```
<img width="671" height="132" alt="image" src="https://github.com/user-attachments/assets/2d0b7253-f22c-4d29-92c2-00bc9db126ac" />

## step 2: Create the directory: Ensure the /opt/docker directory exists:
```
ls -l /opt/
```
<img width="719" height="118" alt="image" src="https://github.com/user-attachments/assets/4ae7d505-605c-457f-b055-9b4a6bff66ed" />

- if docker directory is absent use the below command to create it
```
sudo mkdir -p /opt/docker
```

## step 3: Create the Dockerfile: Use a text editor to create:
```
vi Dockerfile
```
<img width="495" height="20" alt="image" src="https://github.com/user-attachments/assets/5e14d096-b6ab-48d4-b7b4-d52e7b681513" />

```
FROM ubuntu:24.04
# Install apache2
RUN apt-get update && apt-get install -y apache2
# Configure Apache to listen on port 3000
RUN sed -i 's/Listen 80/Listen 3000/' /etc/apache2/ports.conf
RUN sed -i 's/<VirtualHost \*:80>/<VirtualHost \*:3000>/' /etc/apache2/sites-available/000-default.conf
# Expose port 3000
EXPOSE 3000
# Start Apache in the foreground
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
```
**Note :** 
    Explanation of the Dockerfile

    - Base Image: FROM ubuntu:24.04   -----> specifies Ubuntu 24.04 as the base image.
    - Install Apache2: RUN apt-get update && apt-get install -y apache2 ------> updates the package index and installs apache2 non-interactively.
    - Configure Port 3000:
    - sed -i ‘s/Listen 80/Listen 3000/’ /etc/apache2/ports.conf -------> changes the default listening port from 80 to 8083 in the Apache configuration.
    - sed -i ‘s/<VirtualHost \*:80>/<VirtualHost \*:3000>/’ /etc/apache2/sites-available/000-default.conf  -----> updates the virtual host to use port 3000, ensuring Apache listens on all interfaces (*) for port 3000.
    - Expose Port: EXPOSE 8083    -----> documents that the container listens on port 8083.
    - Start Apache: CMD [“/usr/sbin/apache2ctl”, “-D”, “FOREGROUND”] ------> runs Apache in the foreground to keep the container running.

## step 4: Verify the Dockerfile:
```
cat /opt/docker/Dockerfile
```
<img width="953" height="213" alt="image" src="https://github.com/user-attachments/assets/5d94be6c-d7c0-4ea9-b69d-1edcd36d3570" />

- Optional: Build and Test the Image. To ensure the Dockerfile works as expected, you can build and test the image (though not explicitly requested, this verifies the setup).
  Build the image:
  ```
  1. cd /opt/docker
  docker build -t custom-apache:8083 .
  ```
  <img width="1011" height="360" alt="image" src="https://github.com/user-attachments/assets/151851db-3886-4620-8d31-cc6a2544ab9b" />

  2. Run a container:
  ```
  docker run -d -p 3000:3000 custom-apache:8083
  
  <img width="1232" height="119" alt="image" src="https://github.com/user-attachments/assets/8845baad-0a71-4add-bc51-e0108703ad75" />

  3. Test Apache:
  ```
  curl http://localhost:8083
  ```
  
  This should return the default Apache page, confirming Apache is running on port 8083.

  <img width="1144" height="297" alt="image" src="https://github.com/user-attachments/assets/e2b26899-7172-49f3-aef6-62656bdea155" />

  
  

