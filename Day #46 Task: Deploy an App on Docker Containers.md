# Day #46 task: Deploy an App on Docker Containers
The Nautilus Application development team recently finished development of one of the apps that they want to deploy on a containerized platform. The Nautilus Application development and DevOps teams met to discuss some of the basic pre-requisites and requirements to complete the deployment. The team wants to test the deployment on one of the app servers before going live and set up a complete containerized stack using a docker compose fie. Below are the details of the task:

On App Server 2 in Stratos Datacenter create a docker compose file /opt/itadmin/docker-compose.yml (should be named exactly).

The compose should deploy two services (web and DB), and each service should deploy a container as per details below:

**For web service:**

a. Container name must be php_host.

b. Use image php with any apache tag. Check here for more details.

c. Map php_host container's port 80 with host port 8088

d. Map php_host container's /var/www/html volume with host volume /var/www/html.

**For DB service:**

a. Container name must be mysql_host.

b. Use image mariadb with any tag (preferably latest). Check here for more details.

c. Map mysql_host container's port 3306 with host port 3306

d. Map mysql_host container's /var/lib/mysql volume with host volume /var/lib/mysql.

e. Set MYSQL_DATABASE=database_host and use any custom user ( except root ) with some complex password for DB connections.


After running docker-compose up you can access the app with curl command curl <server-ip or hostname>:8088/

For more details check here.

Note: Once you click on FINISH button, all currently running/stopped containers will be destroyed and stack will be deployed again using your compose file.

- Infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details



## step 1 : ssh into the app server and switch to root
```
ssh steve@stapp02
```
<img width="680" height="113" alt="image" src="https://github.com/user-attachments/assets/957f82a2-36fa-4b1f-9cb6-c5ff9d1deea8" />

## step 2 : Ensure the /opt/itadmin directory exists, if not, create it
```
 cd /opt/itadmin
```
<img width="608" height="67" alt="image" src="https://github.com/user-attachments/assets/dc95193f-d3b2-4204-982f-18510c04a533" />

## step 3 : Create the docker-compose.yml file in specified path
```
vi docker-compose.yml
```
```
</html>[steve@stapp02 dba]$ cat docker-compose.yml
version: '3.8'
services:
 web:
  image: php:apache
  container_name: php_blog
  ports:
   - "6400:80"
  volumes:
   - /var/www/html:/var/www/html
  depends_on:
   - db
 db:
  image: mariadb:latest
  container_name: mysql_blog
  ports:
   - "3306:3306"
  volumes:
   - /var/lib/mysql:/var/lib/mysql
  environment:
   - MYSQL_DATABASE=database_blog
   - MYSQL_USER=appuser
   - MYSQL_PASSWORD=Str0ngP@ssw0rd!
   - MYSQL_RANDOM_ROOT_PASSWORD=yes
```

**note**

 <img width="766" height="428" alt="image" src="https://github.com/user-attachments/assets/d333d21f-b6c3-43c7-96c0-1b10a7f58f36" />


- So when the MySQL container starts, it will roughly initialize:
```
MySQL
├── root
│   └── password: RootSecurePass456!
│
├── app_user
│   └── password: SecurePass123!
│
└── database_apache
```

## step 4 : Confirm the file exists and contains the correct content:
```
cat /opt/itadmin/docker-compose.yml
```
 <img width="466" height="382" alt="image" src="https://github.com/user-attachments/assets/1a122e42-8ff3-4088-82b2-ff0966acf8f7" />


## step 5 : Ensure Docker and Docker Compose are installed. Verify Docker is running, and Verify Docker Compose is installed
```
sudo systemctl status docker
```
<img width="796" height="349" alt="image" src="https://github.com/user-attachments/assets/61c66041-2cc9-437b-aab7-c54034e0b8f1" />

```
sudo systemctl start docker
```
```
docker compose version
```
<img width="543" height="31" alt="image" src="https://github.com/user-attachments/assets/c3f4b312-8ac3-4429-9d88-a5be29614df7" />

## step 6 : Run the stack in detached mode
```
docker compose up -d
```
<img width="1249" height="138" alt="image" src="https://github.com/user-attachments/assets/eef161f4-717f-4445-9d80-b6d913c32390" />

## step 7 : Verify the containers. Check that both containers (php_apache and mysql_apache) are running:
```
docker PS
```
<img width="1114" height="161" alt="image" src="https://github.com/user-attachments/assets/a8f374f3-972b-4e1f-a010-4ec1a76aa492" />

## step 8 : Test the web service. Access the application using curl
```
curl http://localhost:6400/
```
<img width="594" height="129" alt="image" src="https://github.com/user-attachments/assets/8cae8c22-2aee-477d-acd5-76d3734c55bd" />



# Explanation of the Docker Compose File
- Version: version: ‘3.8’ ensures compatibility with modern Docker Compose features.

- Web Service:

 - Image: php:8.2-apache is used as a stable PHP image with Apache (an Apache tag as requested).
 - Container Name: php_apache as specified.
 - Ports: Maps host port 6400 to container port 80 (6400:80).
 - Volumes: Maps host directory /var/www/html to container directory /var/www/html for persistent web content.
 - Depends On: Ensures the db service is started before the web service.

- DB Service:

 - Image: mariadb:latest is used as the MariaDB image (preferred latest tag).
 - Container Name: mysql_apache as specified.
 - Ports: Maps host port 3306 to container port 3306 (3306:3306).
 - Volumes: Maps host directory /var/lib/mysql to container directory /var/lib/mysql for persistent database data.
 - Environment Variables:

  - MYSQL_DATABASE=database_apache: Creates a database named database_apache.
  - MYSQL_USER=appuser: Creates a custom user (not root) for DB connections.
  - MYSQL_PASSWORD=Str0ngP@ssw0rd!: Sets a complex password for the appuser.
  - MYSQL_RANDOM_ROOT_PASSWORD=yes Docker will automatically generate a random password for the MySQL root user when the container is created.




