# Day 19 task: Install and Configure Web Application

xFusionCorp Industries is planning to host two static websites on their infra in Stratos Datacenter. The development of these websites is still in-progress, but we want to get the servers ready. Please perform the following steps to accomplish the task:



a. Install httpd package and dependencies on app server 3.


b. Apache should serve on port 6000.


c. There are two website's backups /home/thor/beta and /home/thor/demo on jump_host. Set them up on Apache in a way that beta should work on the link http://localhost:6000/beta/ and demo should work on link http://localhost:6000/demo/ on the mentioned app server.


d. Once configured you should be able to access the website using curl command on the respective app server, i.e curl http://localhost:6000/beta/ and curl http://localhost:6000/demo/


## step1: ssh to app server in which apche instalation required.
(in above case app-server3)
```
ssh banner@stapp03
```
check for the os distribution and apache status
```
cat /etc/os-release/
```
```
sudo systemctl status https
```
if apache is not there, need to install it, enable, start and see the status
```
sudo yum install -y httpd
```
```
sudo systemctl enable httpd
```
```
sudo systemctl start httpd
```
```
sudo systemctl status httpd
```
## step2: check on which port apche is listening, and edit according to problem statement. save and exit.
```
cat /etc/httpd/conf/httpd.conf | grep Listen*
```
```
sudo vi /etc/httpd/conf/httpd.conf
```

## step3: Copy Website Files 
Copy the backups from jump_host to the app server.
From the jump_host, run:
```
scp -r /tmp/thor/beta bannet@stapp03:/tmp/
```
```
scp -r /tmp/thor/demo banner@stapp03:/tmp/
```
## step4: move the files to proper location on App Server 3
```
sudo mv /tmp/beta /var/www/html/
```
```
sudo mv /tmp/demo /var/www/html/
```
## step5: Set proper Permissions so that it will execute
(change of owner and file permissions are required for the execution)
```
sudo chown -R apache:apache /var/www/html/beta /var/www/html/demo
```
```
sudo chmod -R 755 /var/www/html/beta /var/www/html/demo
```
## step6: Configure Virtual Hosts
Although both can work directly under /var/www/html, it’s cleaner to create directory aliases.
Edit the Apache config file again:
```
sudo vi /etc/httpd/conf/httpd.conf
```
At the bottom, add:
```
Alias /beta /var/www/html/beta
<Directory /var/www/html/beta>
    AllowOverride None
    Require all granted
</Directory>

Alias /demo /var/www/html/demo
<Directory /var/www/html/demo>
    AllowOverride None
    Require all granted
</Directory>
```

save and exit.

## step7: Start and Enable Apache

```
sudo systemctl enable httpd
```
```
sudo systemctl restart httpd
```
## Step8: Test Configuration
```
curl http://localhost:8084/beta/
```
```
curl http://localhost:8084/demo/
```
You should see the HTML output of both sites.




