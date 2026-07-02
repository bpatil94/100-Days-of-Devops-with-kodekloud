# Day 20 task: Configure Nginx + PHP-FPM Using Unix Sock

The Nautilus application development team is planning to launch a new PHP-based application, which they want to deploy on Nautilus infra in Stratos DC. The development team had a meeting with the production support team and they have shared some requirements regarding the infrastructure. Below are the requirements they shared:



a. Install nginx on app server 3 , configure it to use port 8097 and its document root should be /var/www/html.


b. Install php-fpm version 8.3 on app server 3, it must use the unix socket /var/run/php-fpm/default.sock (create the parent directories if don't exist).


c. Configure php-fpm and nginx to work together.


d. Once configured correctly, you can test the website using curl http://stapp03:8097/index.php command from jump host.

NOTE: We have copied two files, index.php and info.php, under /var/www/html as part of the PHP-based application setup. Please do not modify these files.

 ## Note
 What is Nginx?
Nginx is a web server that handles HTTP requests from users’ browsers.
- It serves static files (like HTML, images).
- It can also forward dynamic requests (like PHP) to a processor such as PHP-FPM.

What is PHP-FPM?
PHP-FPM (FastCGI Process Manager) is a PHP implementation designed for web servers like Nginx that cannot process PHP directly.
- It runs as a separate service.
- It handles PHP script execution.
- It manages multiple worker processes for better performance and communicates with the web server using FastCGI.

What is a Unix Sock?
A Unix sock or socket is definitely not a sock, but it is a special file used for inter-process communication (IPC) on the same machine.
- Acts as a private “door” between Nginx and PHP-FPM.
- Access can be controlled via ownership and permissions.
- It is faster than using the internet (TCP) because it stays inside the server.

- <img width="659" height="520" alt="image" src="https://github.com/user-attachments/assets/72b39411-fe9c-4de9-bbdc-73891241401e" />

- Nginx receives an HTTP request for a PHP file (index.php in our problem statement).
- Nginx forwards the request to php-fpm via the Unix socket (/var/run/php-fpm/default.sock).
- php-fpm executes the PHP script and generates HTML output.
- Nginx receives the output and sends it to the client.
- This separation allows Nginx to remain lightweight while PHP-FPM handles all PHP logic efficiently.

  ## step1 : ssh to specified server on which these to be installed
  ( in above i.e app server 3)
  ```
  ssh banner@stapp03
  ```

  ## step2 : install nginx, configure it to use port 8097 and its document root should be /var/www/html
  ```
  sudo yum install nginx -y
  ```
  ```
  sudo vi /etc/nginx/conf
  ```
  Replace the server part of the code with the below code. Please don't touch the TLS server part of the code.
```
 server {
        listen       <given-port>;
        listen       [::]:<given-port>;
        server_name  <app-server-name>;
        root /var/www/html;
        index index.php index.html;

        location / {
           try_files $uri $uri/ =404;
         }

        location ~ \.php$ {
            include fastcgi_params;
            fastcgi_pass unix:/var/run/php-fpm/default.sock;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
         }

        error_page 404 /404.html;
        error_page 500 502 503 504 /50x.html;
}
```
Save the file and enable and start the nginx service. Validate if the service is actually running on the mentioned port.
```
sudo systemctl enable nginx
```
```
sudo systemctl start nginx
```
check on which the nginx is listening
```
sudo ss -tulpn | grep nginx
```
### About these lines
location ~ \.php$ {
            include fastcgi_params;
            fastcgi_pass unix:/var/run/php-fpm/default.sock;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
         }

-location ~ \.php$ { → Handle all requests for .php files.

-include fastcgi_params; → Provide PHP-FPM with standard request info.

-fastcgi_pass unix:/var/run/php-fpm/default.sock; → Send PHP requests to PHP-FPM via the Unix socket.

-fastcgi_index index.php; → Use index.php as the default file for a directory.

-fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name; → Tell PHP-FPM the exact file path to execute. 

## step3: Install PHP-FPM <version-mentioned>
```
sudo dnf install epel-release -y
```
```
sudo dnf install https://rpms.remirepo.net/enterprise/remi-release-9.rpm -y
```
```
sudo dnf module list php
```
```
sudo dnf module enable php:remi-<version-mentioned> -y
```
```
sudo dnf install php-fpm php php-cli php-common php-mysqlnd php-gd php-xml php-mbstring php-pdo php-opcache -y
``
sudo systemctl start php-fpm
```
```
sudo systemctl enable php-fpm
```
Validate if the correct version of php-fpm is installed and that it's running.php -v
```
php -v
```
```
sudo systemctl status php-fpm
```
You need to make a few changes to configure php-fpm
```
sudo vi /etc/php-fpm.d/www.conf
```
<img width="792" height="589" alt="image" src="https://github.com/user-attachments/assets/a7ff32b5-06a8-401e-8b80-31c0773f6e1d" />
```
sudo mkdir -p /var/run/php-fpm
```
```
sudo chown -R nginx:nginx /var/run/php-fpm
```
```
sudo systemctl enable --now php-fpm
```
```
sudo systemctl restart php-fpm
```
Validate if the socket is actually created after restarting the php-fpm service.
try from both side, appserver and from jump server too
```
curl http://stapp02:8092/index.php
```
  







