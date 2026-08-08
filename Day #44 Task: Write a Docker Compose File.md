# Day 44: Write a Docker Compose File
The Nautilus application development team shared static website content that needs to be hosted on the httpd web server using a containerised platform. The team has shared details with the DevOps team, and we need to set up an environment according to those guidelines. Below are the details:

a. On App Server 3 in Stratos DC create a container named httpd using a docker compose file /opt/docker/docker-compose.yml (please use the exact name for file).

b. Use httpd (preferably latest tag) image for container and make sure container is named as httpd; you can use any name for service.

c. Map 80 number port of container with port 3001 of docker host.  

d. Map container's /usr/local/apache2/htdocs volume with /opt/security volume of docker host which is already there. (please do not modify any data within these locations).

-infrastructure details:https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details



## step 1: ssh into the app server
```
ssh banner@stapp03
```
<img width="691" height="131" alt="image" src="https://github.com/user-attachments/assets/66fd245c-d88b-47c8-a6e7-7aa9677691bf" />

## step 2 : Verify the named path '/opt/docker/' is already existed or not
```
ls -l /opt/docker/
```
- if not create it
  ```
  sudo mkdir -p /opt/docker
  ```
  <img width="467" height="175" alt="image" src="https://github.com/user-attachments/assets/e66982f8-9859-48ae-a892-8dbf7c906a09" />

## Step 3: Create the Docker Compose File 
```
sudo vi docker-compose.yml
```
```
version: "3"
services:
  webserver:
    image: httpd:latest
    container_name: httpd
    ports:
      - "3001:80"
    volumes:
      - /opt/security:/usr/local/apache2/htdocs
```
<img width="696" height="227" alt="image" src="https://github.com/user-attachments/assets/a0889056-f365-4c62-bfb0-470490500051" />

## Step 4: Deploy the Container Using Docker Compose

- Start the container in detached mode
```
sudo docker-compose up -d
```
<img width="1185" height="118" alt="image" src="https://github.com/user-attachments/assets/436a613a-b08e-4f44-bd37-54dac8d1099b" />

## Step 5: Verify the Deployment
```
### Check if the container is running
sudo docker ps

<img width="1114" height="115" alt="image" src="https://github.com/user-attachments/assets/1d490467-e72b-408c-a1f7-0ad371a5ad46" />

### Expected output should show the 'httpd' container running

### Verify port mapping
sudo ss -tuln | grep 3001
- if not found ss, use bellow to install
sudo dnf install iproute

<img width="720" height="101" alt="image" src="https://github.com/user-attachments/assets/0eef8c75-eb59-48f9-a967-11f2eeae29aa" />
<img width="575" height="62" alt="image" src="https://github.com/user-attachments/assets/1949df61-c95e-48fc-a809-4b658195266a" />

### Test if the web server is serving content
```
curl http://localhost:8085
```
<img width="820" height="209" alt="image" src="https://github.com/user-attachments/assets/3312d263-9d6b-4b26-b7ac-5b05e9beb53d" />

### check wheather the volume is properly mounted or not ( see the files in docker host "/opt/security/" path those things should reflect in the containers mentioned path "/usr/local/apache2/htdocs")

<img width="570" height="269" alt="image" src="https://github.com/user-attachments/assets/cb73ad6b-e39e-4f00-b5ef-b37c5301afd0" />

======================================================================================= ===================================================== ==================================================


# Common Docker Compose Commands
```
# Start containers
docker-compose up -d

# Stop containers
docker-compose down

# View logs
docker-compose logs

# Restart containers
docker-compose restart

# Check status
docker-compose ps

```

# Troubleshooting

If you encounter any issues:

1.Permissions Issues:
```
# Check permissions on /opt/data
ls -la /opt/data

# Fix permissions if needed
sudo chmod -R 755 /opt/data
```
2. Port Already in Use:
```
# Check if port is already in use
sudo ss -tuln | grep 8085

# Find and stop the process using the port
sudo fuser -k 8085/tcp
```

3. Docker Compose Version Issues:
```
# Check Docker Compose version
docker-compose --version

# If version is too old, you may need to update the version in the yaml file
```


# Verification

After deployment, the Apache HTTP Server should be running in a container named httpd, accessible via port 8085 on the host, and serving content from the /opt/data directory.

To verify everything is working correctly:
```
# Check container status
sudo docker ps | grep httpd

# Access the website
curl -I http://localhost:8085
```

