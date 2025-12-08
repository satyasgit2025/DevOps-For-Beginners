The application development team is planning to launch a new PHP-based application, which they want to deploy in DC. 
The development team had a meeting with the production support team and they have shared some requirements regarding the infrastructure.
Below are the requirements they shared:
a. Install nginx on app server, configure it to use port 8099 and its document root should be /var/www/html.
b. Install php-fpm version 8.1 on app server, it must use the unix socket /var/run/php-fpm/default.sock (create the parent directories if don't exist).
c. Configure php-fpm and nginx to work together.
d. Once configured correctly, you can test the website using curl http://pp03:8099/index.php command from jump host.

Task: Configure Nginx With PHP-FPM 8.1 on App Server
Objective

Set up Nginx on the application server using:

Port 8099

Document root /var/www/html

PHP-FPM 8.1

PHP-FPM socket at /var/run/php-fpm/default.sock

Connect Nginx with PHP-FPM

Validate application via:

curl http://app:8099/index.php

Steps Performed
1. Enable and install PHP 8.1 & PHP-FPM
```
sudo dnf module reset php -y
sudo dnf module enable php:8.1 -y
sudo dnf install php php-fpm php-cli php-common php-mysqlnd -y
```

Validation:
```
php -v
php-fpm -v
```

2. Configure PHP-FPM socket

Edit pool file:
```
sudo vi /etc/php-fpm.d/www.conf

Update:

listen = /var/run/php-fpm/default.sock
listen.owner = nginx
listen.group = nginx
listen.mode = 0660
```

Create socket directory:
```
sudo mkdir -p /var/run/php-fpm
sudo chown -R nginx:nginx /var/run/php-fpm
```

Restart service:
```
sudo systemctl enable php-fpm --now
```
3. Install and configure Nginx
```
sudo dnf install nginx -y
```

Edit default server block:
```
sudo vi /etc/nginx/nginx.conf


Add inside server block:

listen 8099;
root /var/www/html;
index index.php index.html;

location ~ \.php$ {
    fastcgi_pass unix:/var/run/php-fpm/default.sock;
    fastcgi_index index.php;
    include fastcgi.conf;
}
```

Test and restart:
```
sudo nginx -t
sudo systemctl enable nginx --now
```
4. Validation

Test from jump host:
```
curl http://stapp03:8099/index.php
```


Expected output:

Welcome!


And full phpinfo() page for info.php.

Both validated successfully.

Final Result

✔ Nginx installed on port 8099
✔ Document root correctly set
✔ PHP-FPM 8.1 installed and running
✔ Socket configured: /var/run/php-fpm/default.sock
✔ Application accessible from jump host
✔ Task completed successfully

