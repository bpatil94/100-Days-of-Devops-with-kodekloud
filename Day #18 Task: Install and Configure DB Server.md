# Day 18 task :Install and Configure DB Server
We need to setup a database server on Nautilus DB Server in Stratos Datacenter. Please perform the below given steps on DB Server:

a. Install/Configure MariaDB server.

b. Create a database named kodekloud_db7.

c. Create a user called kodekloud_joy and set its password to LQfKeWWxWD.

d. Grant full permissions to user kodekloud_joy on database kodekloud_db7.

## step1: log in to database server
```
ssh databaseserver@password
```
## step2: check for the os distribution
```
cat /etc/os-release
```
## step3: install mariadb ( use yum for cent os and apt-get for ubuntu)
```
sudo yum install -y mariadb-service
```
## step4: eaable, start and check for the status of mariadb
```
sudo systemctl enable mariadb
```
sudo systemctl start mariadb
```
sudo systemctl status mariadb
```
## step5: Create a database named kodekloud_db7
Login to the DB as root user:
```
sudo mysql
```

