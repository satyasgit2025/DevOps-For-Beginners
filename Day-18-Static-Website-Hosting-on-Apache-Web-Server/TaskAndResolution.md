Company is planning to host two static websites on their infra in Datacenter. The development of these websites is still in-progress, but we want to get the servers ready. Please perform the following steps to accomplish the task:

a. Install httpd package and dependencies on app server.

b. Apache should serve on port 3003.

c. There are two website's backups /home/jumphost/media and /home/jumphost/apps on jump_host. Set them up on Apache in a way that media should work on the link http://localhost:3003/media/ and apps should work on link http://localhost:3003/apps/ on the mentioned app server.

d. Once configured you should be able to access the website using curl command on the respective app server, i.e curl http://localhost:3003/media/ and curl http://localhost:3003/apps/


Tasks Performed
1. Installed Apache (httpd) on App Server
```
sudo yum install httpd -y
sudo systemctl enable httpd
```
_____________________________________
2. Configured Apache to listen on port 3003
Edited Apache configuration:
```
sudo vi /etc/httpd/conf/httpd.conf
Replaced:
Listen 80
With:
Listen 3003
```
________________________________________
3. Transferred Website Data from Jump Host
SSH into jump_host:
```
ssh user@jumphost
Copied website backups:
scp -r /home/thor/media banner@172.16.238.12:/tmp/
scp -r /home/thor/apps banner@172.16.238.12:/tmp/
```
Logged into App Server 3:
Moved content into Apache document root:
```
sudo mkdir -p /var/www/html/media
sudo mkdir -p /var/www/html/apps
sudo cp -r /tmp/media/* /var/www/html/media/
sudo cp -r /tmp/apps/*  /var/www/html/apps/
```
________________________________________
4. Apache Alias Configuration for Media & Apps Websites
Created a dedicated config file:
```
sudo vi /etc/httpd/conf.d/static-sites.conf
```
Added the following:
```
Alias /media /var/www/html/media
<Directory /var/www/html/media>
    AllowOverride None
    Require all granted
</Directory>

Alias /apps /var/www/html/apps
<Directory /var/www/html/apps>
    AllowOverride None
    Require all granted
</Directory>
```
________________________________________
5. Fixed Incorrect Directory Structure
Initially, Apache showed folder listings because content was nested like:
/var/www/html/media/media/
/var/www/html/apps/apps/
Resolved by flattening directories:
```
sudo mv /var/www/html/media/media/* /var/www/html/media/
sudo rmdir /var/www/html/media/media
sudo mv /var/www/html/apps/apps/* /var/www/html/apps/
sudo rmdir /var/www/html/apps/apps
```
Updated permissions:
```
sudo chown -R apache:apache /var/www/html/
```

_____________________________________
6. Verification Using curl
Media website
```
curl http://localhost:3003/media/
```
Output:
<!DOCTYPE html>
<html>
<body>
<h1>TechFusion</h1>
<p>This is a sample page for our media website</p>
</body>
</html>
Apps website

curl http://localhost:3003/apps/
Output:
<!DOCTYPE html>
<html>
<body>
<h1>SkyApps</h1>
<p>This is a sample page for our apps website</p>
</body>
</html>
Note
A restart was not required because only content changed, not Apache configuration.
________________________________________
Final Result
✔ Apache installed and enabled
✔ Serving content on port 3003
✔ /media and /apps websites hosted successfully
✔ Website data from backup integrated correctly
✔ Verified using curl
✔ Ready for staging & deployment

