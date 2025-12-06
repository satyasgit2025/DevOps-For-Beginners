Web Application Deployment – Task \& Resolution

Task Overview



The objective is to configure a complete LAMP-based environment consisting of:



Installing and configuring Apache and PHP on all application servers



Running Apache on a custom port (5000)



Installing and configuring MariaDB on the database server



Creating a database and a database user with full privileges



Verifying database connectivity



Creating a PHP-based DB connection test page to validate access through both:



Local Apache test



Load Balancer (browser)



✅ Resolution Steps

Step 1: Install Apache, PHP \& dependencies on all application servers

sudo yum install -y httpd php php-mysqlnd php-cli php-gd php-xml



Step 2: Configure Apache to run on port 5000



Edit the Apache configuration file:



sudo vi /etc/httpd/conf/httpd.conf





Update the port:



Listen 5000





Modify or add a VirtualHost:



<VirtualHost \*:5000>

&nbsp;   DocumentRoot "/var/www/html"

&nbsp;   DirectoryIndex index.php index.html

</VirtualHost>



Step 3: Start, enable \& test Apache locally

sudo systemctl enable httpd

sudo systemctl restart httpd

curl http://localhost:5000



Step 4: Install \& configure MariaDB on the database server

sudo yum install -y mariadb-server

sudo systemctl enable mariadb

sudo systemctl start mariadb



Step 5: Create database, user, and grant privileges



Log into MariaDB:



sudo mysql





Run SQL commands:



CREATE DATABASE app\_database;



CREATE USER 'app\_user'@'%' IDENTIFIED BY 'StrongPassword123';



GRANT ALL PRIVILEGES ON app\_database.\* TO 'app\_user'@'%';



FLUSH PRIVILEGES;

EXIT;



Step 6: Verify database connectivity from the application server

mysql -h db-server -u app\_user -p





Test commands:



USE app\_database;

SHOW TABLES;



Step 7: Create a PHP file to test database connectivity (local + load balancer)

cd /var/www/html

sudo vi index.php





Insert:



<?php

$servername = "db-server";

$username = "app\_user";

$password = "StrongPassword123";

$dbname = "app\_database";



$conn = new mysqli($servername, $username, $password, $dbname);



if ($conn->connect\_error) {

&nbsp;   die("Connection failed: " . $conn->connect\_error);

}



echo "Application is able to connect to the database using user app\_user";

?>





Restart Apache:



sudo systemctl restart httpd





Test locally:



curl http://localhost:5000





Expected output:



Application is able to connect to the database using user app\_user



🎯 Outcome



Apache successfully runs on port 5000



MariaDB is installed and operational



Database app\_database and user app\_user (with full privileges) are created



Application connectivity is confirmed both:



Locally via curl



Remotely through the Load Balancer



The environment is now ready for hosting PHP-based or WordPress-style applications.

