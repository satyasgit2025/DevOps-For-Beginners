✅ 1. Install Nginx on App Server after that Enable,start & check the Nginx.
```
sudo yum install -y epel-release
sudo yum install -y nginx
sudo systemctl enable nginx
sudo systemctl start nginx
sudo systemctl status nginx
```
✅ 2. Move SSL Certificate & Key to proper location from /tmp to /etc/nginx/ssl/ (Move them to /etc/nginx/ssl/).
```
sudo mkdir -p /etc/nginx/ssl
sudo mv /tmp/xyz.crt /etc/nginx/ssl/
sudo mv /tmp/xyz.key /etc/nginx/ssl/
```
-Set correct permissions:
```
sudo chmod 600 /etc/nginx/ssl/nautilus.key
sudo chmod 644 /etc/nginx/ssl/nautilus.crt
```
✅ 3. Configure Nginx to use SSL sdit the default Nginx SSL configuration.
```
sudo vi /etc/nginx/conf.d/ssl.conf
```
-Add the following configuration:
```
server {
    listen 443 ssl;
    server_name _;

    ssl_certificate     /etc/nginx/ssl/xyz.crt;
    ssl_certificate_key /etc/nginx/ssl/xyz.key;

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
✅ 4. Test Nginx configuration & Reload.
```
sudo nginx -t
sudo systemctl reload nginx
```
✅ 5. Create the index.html under Nginx document root & Check the same.
```
cd /usr/share/nginx/html/
echo "Welcome!" | sudo tee /usr/share/nginx/html/index.html
cat /usr/share/nginx/html/index.html
```
✅ 6. Final Testing from the Jump Host
```
curl -Ik https://<appserver-ip>/
```
-Expected output:
```
HTTP/1.1 200 OK
Server: nginx
```






