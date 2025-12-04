✅ Step 1 — SSH into LBR Host at which need configure nginx.
```
sudo yum install -y epel-release
sudo yum install -y nginx
sudo systemctl enable nginx
sudo systemctl start nginx
sudo systemctl status nginx
```
✅ Step 3 — Configure Nginx Load Balancing Update ONLY the main configuration file.
```
cd /etc/nginx/
sudo vi /etc/nginx/nginx.conf
```
-Inside the http { } block, add an upstream + a server block
```
http {
    upstream backend_app {
        server 172.16.238.10:5003;
        server 172.16.238.11:5003;
        server 172.16.238.12:5003;
    }

    server {
        listen 80;
        location / {
            proxy_pass http://backend_app;
        }
    }
}
```
✅ Step 4 — Test & Nginx configuration
```
sudo nginx -t

-Output
-syntax is ok
-test is successful

-Reload
```
sudo systemctl reload nginx

✅ Step 5 — Verify Apache is running on all app servers
```
sudo systemctl status httpd
sudo systemctl status httpd
```
✅ Step 6 — Test Using LB IP.
```
curl -I http://<LB-IP>/
```
✅ Step 7 — /etc/nginx/nginx.conf
```
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 4096;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    include /etc/nginx/conf.d/*.conf;

    # 🔥 Upstream load balancing
    upstream backend_app {
        server 172.16.238.10:5003;
        server 172.16.238.11:5003;
        server 172.16.238.12:5003;
    }

    # 🔥 Front-end LB server block
    server {
        listen       80;
        listen       [::]:80;
        server_name  _;

        location / {
            proxy_pass http://backend_app;
        }

        error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }
    }
}




