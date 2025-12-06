# Web Application Deployment – Task & Resolution

## **Task Overview**

The objective is to configure a complete LAMP-based environment consisting of:

1. Installing and configuring **Apache** and **PHP** on all application servers  
2. Running Apache on a **custom port (5000)**  
3. Installing and configuring **MariaDB** on the database server  
4. Creating a database and a database user with full privileges  
5. Verifying database connectivity  
6. Creating a PHP-based DB connection test page to validate access through:  
   - Local Apache test  
   - Load Balancer (browser)

---

# ✅ Resolution Steps

---

## **Step 1: Install httpd, PHP & dependencies on all application servers**

```
sudo yum install -y httpd php php-mysqlnd php-cli php-gd php-xml
```
## **Step 2: Configure Apache to run on port 5000**

Edit Apache configuration:
```
sudo vi /etc/httpd/conf/httpd.conf
```

Update: Listen 5000

Add or modify VirtualHost:
```
<VirtualHost *:5000>
    DocumentRoot "/var/www/html"
    DirectoryIndex index.php index.html
</VirtualHost>
```
## **Step 3: Start, enable & test Apache locally**
```
sudo systemctl enable httpd
sudo systemctl restart httpd
curl http://localhost:5000
```
## **Step 4: Install & configure MariaDB on the database server**
```
sudo yum install -y mariadb-server
sudo systemctl enable mariadb
sudo systemctl start mariadb
```
## **Step 5: Create database, user, and assign privileges**

Log into MariaDB:
```
sudo mysql
```
Run SQL:
```
CREATE DATABASE app_database;

CREATE USER 'app_user'@'%' IDENTIFIED BY 'StrongPassword123';

GRANT ALL PRIVILEGES ON app_database.* TO 'app_user'@'%';

FLUSH PRIVILEGES;
EXIT;
```
## **Step 6: Verify database connectivity from the application server**
```
mysql -h db-server -u app_user -p
```
Run:
```
USE app_database;
SHOW TABLES;
```
## **Step 7: Create the PHP file to test database connectivity (via Load Balancer and locally)**

Navigate to the document root:
```
cd /var/www/html
sudo vi index.php
```
Insert:
```
<?php
$servername = "db-server";
$username = "app_user";
$password = "StrongPassword123";
$dbname = "app_database";

$conn = new mysqli($servername, $username, $password, $dbname);

if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

echo "Application is able to connect to the database using user app_user";
?>
```
# Restart Apache:
```
sudo systemctl restart httpd
```
# Test locally:
```
curl http://localhost:5000
```
Expected output:

Application is able to connect to the database using user app_user


🎯 Outcome

Apache successfully runs on port 5000

MariaDB is installed and operational

Database app_database and user app_user created with full privileges

Connectivity verified via:

Local Apache (curl)

Load Balancer (browser)

======================End=======================================
