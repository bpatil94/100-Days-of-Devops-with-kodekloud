# Day 47: Docker Python App

A python app needed to be Dockerized, and then it needs to be deployed on App Server 3. We have already copied a requirements.txt file (having the app dependencies) under /python_app/src/ directory on App Server 3. Further complete this task as per details mentioned below:

 1. Create a Dockerfile under /python_app directory:

   Use any python image as the base image.
   Install the dependencies using requirements.txt file.
   Expose the port 6100.
   Run the server.py script using CMD.

 2. Build an image named nautilus/python-app using this Dockerfile.


 3. Once image is built, create a container named pythonapp_nautilus:

    Map port 6100 of the container to the host port 8094.

 4. Once deployed, you can test the app using curl command on App Server 3.
    curl http://localhost:8094/


- Infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details

## step 1: ssh to server and cd to mentioned path
```
ssh steve@stapp02
```
<img width="357" height="36" alt="image" src="https://github.com/user-attachments/assets/b8de2bf7-61f3-446a-98a5-b6e47cf51496" />

```
cd /python_app
```
<img width="560" height="146" alt="image" src="https://github.com/user-attachments/assets/144543e5-977d-4d8b-a791-970c26a43c8e" />

## step 2 : Create the Dockerfile
```
vi Dockerfile
```

Insert the contents
 ```
  # Use a Python 3.9 slim image as the base
  FROM python:3.9-slim

  # Set the working directory inside the container
  WORKDIR /app

  # Copy the requirements.txt file from the src/ directory
  COPY src/requirements.txt .

  # Install the dependencies
  RUN pip install --no-cache-dir -r requirements.txt

  # Copy the application source code from the src/ directory
  COPY src/ .

  # Expose port 6100
  EXPOSE 6100

  # Run the server.py script when the container starts
  CMD ["python", "server.py"]
```
<img width="460" height="339" alt="image" src="https://github.com/user-attachments/assets/eea8c37e-401a-4f90-ab60-95f77c23ce8c" />

```
Docker build
     |
     v
FROM python:3.11-slim
     |
     v
Create /app
     |
     v
Copy requirements.txt
     |
     v
Install Python dependencies
     |
     v
Copy application source code
     |
     v
Create Docker image

```

## step 3 : Build the Docker Image
```
docker build -t nautilus/python-app .
```
<img width="770" height="325" alt="image" src="https://github.com/user-attachments/assets/2736a2eb-f8c0-436b-98cb-1e9081f3c180" />

## step 4 : Run the Docker Container
```
docker run -d --name pythonapp_nautilus -p 8094:6100 nautilus/python-app
```
<img width="1090" height="84" alt="image" src="https://github.com/user-attachments/assets/39673915-11fb-4ec9-8ee4-10deac4fd189" />

```
Container starts
      |
      v
python server.py
      |
      v
Application listens on :6100
      |
      v
-p 6300:6300
      |
      v
Application accessible from host on port 6100
```
## step 5 : Test the Application from stapp03 as well as from jumphost
- from stapp03
```
curl http://localhost:8094/
```
<img width="491" height="38" alt="image" src="https://github.com/user-attachments/assets/64cac874-649e-46f2-b70c-ff50a3d56f5c" />

- from Jumphost
```
curl http://stapp03:8094
```
<img width="575" height="121" alt="image" src="https://github.com/user-attachments/assets/1de0302f-98db-4142-8e3f-4815fd125a69" />

  

