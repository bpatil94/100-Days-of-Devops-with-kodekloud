# Day 40: Docker EXEC Operations

One of the Nautilus DevOps team members was working to configure services on a kkloud container that is running on App Server 2 in Stratos Datacenter. Due to some personal work he is on PTO for the rest of the week, but we need to finish his pending work ASAP. Please complete the remaining work as per details given below:


a. Install apache2 in kkloud container using apt that is running on App Server 2 in Stratos Datacenter.


b. Configure Apache to listen on port 3004 instead of default http port. Do not bind it to listen on specific IP or hostname only, i.e it should listen on localhost, 127.0.0.1, container ip, etc.


c. Make sure Apache service is up and running inside the container. Keep the container in running state at the end.

- Infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details

## 1. SSH into App Server 1
```
ssh steve@stapp02
```

<img width="748" height="366" alt="image" src="https://github.com/user-attachments/assets/6bd58415-04ee-450d-b2cc-b1fca0313c19" />


## 2. Access the running container
```
docker ps
```
to list out all, running and stopped containers use below cmd
```
docker ps -a
```

<img width="821" height="122" alt="image" src="https://github.com/user-attachments/assets/4c1e2584-411a-4fa9-bbf0-57cc8ade0597" /> 

## 3. Update packages and install Apache2
First, enter the container shell:

```
docker exec -it kkloud bash
```

<img width="736" height="60" alt="image" src="https://github.com/user-attachments/assets/3cf4f634-b9e7-4bb7-8d22-204a50af6beb" /> 

Once inside the container shell, update package lists and install Apache:
```
apt update
```
```
apt install apache2
```

<img width="623" height="190" alt="image" src="https://github.com/user-attachments/assets/0cea8ead-983a-45bf-97ff-99c005fbb5a5" />


You can verify Apache installation with:

```
apache2 -v
```
<img width="482" height="53" alt="image" src="https://github.com/user-attachments/assets/869c10c0-1d60-4d87-a938-294413a83b87" /> 

## 4. Start the Apache service
```
service apache2 start
```

<img width="437" height="53" alt="image" src="https://github.com/user-attachments/assets/55cc85b1-28a7-4ce5-b950-61f1c13c00dd" />

## 5. Change Apache listening port
- By default, Apache listens on port 80. Reconfigure it to listen on 3004 (as per the requirements) by editing the ports configuration file:

vim was not there in running container, so i installed vim
```
apt install -y vim
```

<img width="353" height="20" alt="image" src="https://github.com/user-attachments/assets/864698b9-85e9-4957-a577-4fbccfb07be5" />

```
vim /etc/apache2/ports.conf
```

<img width="460" height="22" alt="image" src="https://github.com/user-attachments/assets/54231306-25cc-4492-82ef-a5e724f10ece" />

Find the line:
Listen 80

Change it to:
Listen 3004

- Next, update the default virtual host configuration:

  A default virtual host is the website that a web server serves when an incoming request does not match any configured virtual host (or when it's the first virtual host loaded).

 ** Why is it needed? **

  A single web server can host multiple websites using Virtual Hosts (Apache) or Server Blocks (Nginx). The default virtual host acts as a fallback.


```
  vim /etc/apache2/sites-enabled/000-default.conf
```


<img width="583" height="18" alt="image" src="https://github.com/user-attachments/assets/b96d0146-70a4-4885-bacd-cbf88bb3070e" />


Locate the line:
<VirtualHost *:80>


and change it to:
<VirtualHost *:5003>

Save and exit.

## 6. Restart Apache to apply changes and verify
```
service apache2 restart
```

<img width="467" height="41" alt="image" src="https://github.com/user-attachments/assets/bc1e6e8f-a88f-425e-b9f1-03bb9d7547fc" />

Now confirm it’s running:

```
service apache2 status
```
Check that it’s listening on port 3004:

```
netstat -tuln | grep 5003
```

<img width="386" height="93" alt="image" src="https://github.com/user-attachments/assets/e93a31e7-7467-4221-a655-7fa8a79c4748" />


or, if netstat is unavailable:
```
ss -tuln | grep 5003
```

- If none of the packages are installed in the container, install either of them using:

```
  apt install net-tools # To install netstat
  apt install iproute2  # To install ss (socket statistics)
```

<img width="806" height="40" alt="image" src="https://github.com/user-attachments/assets/3b4ff1bd-3148-44fe-9abd-31160f81deda" />

## 6. Keep the container running
Exit the container shell, but make sure it remains active:

```
exit
```
```
docker ps
```
If the container runs in interactive mode and stops on exit, restart it detached:
```
docker start kkloud
```

<img width="824" height="94" alt="image" src="https://github.com/user-attachments/assets/4dc869af-673b-4ed9-ade9-c8abc9fef038" />


Note: This task reinforces an important DevOps concept: service configuration within containers. Even though containers are lightweight and disposable, you can still modify configurations inside them for debugging or legacy setups.

However, in production pipelines, such configurations are better handled using Dockerfiles and environment variables for consistency and reproducibility.






