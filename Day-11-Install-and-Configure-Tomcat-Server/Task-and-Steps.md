✅ Task Summary – Tomcat Installation & Application Deployment on App Server 2
Objective:

Install Tomcat on App Server 2, configure it to run on port 6100, and deploy the provided ROOT.war application from the Jump Host.

1️⃣ Transferred ROOT.war from Jump Host to App Server 2
On the Jump Host:
```
scp -o StrictHostKeyChecking=no /tmp/ROOT.war steve@stapp02.stratos.xfusioncorp.com:/tmp/


2️⃣ Logged into App Server 2:
```
ssh steve@stapp02.stratos.xfusioncorp.com
sudo -i

3️⃣ Installed Tomcat & Java:
```
yum install -y java-1.8.0-openjdk
yum install -y tomcat tomcat-webapps tomcat-admin-webapps


4️⃣ Configured Tomcat to run on port 6100
Edited server.xml & update the connector port form 8080 to 6100 "<Connector port="6100" protocol="HTTP/1.1":
```
vi /etc/tomcat/server.xml


5️⃣ Deployed ROOT.war
Removed old ROOT directory:
```
rm -rf /usr/share/tomcat/webapps/ROOT

Copied the new WAR file:
```
mv /tmp/ROOT.war /usr/share/tomcat/webapps/

Corrected ownership:
```
chown tomcat:tomcat /usr/share/tomcat/webapps/ROOT.war
chmod 644 /usr/share/tomcat/webapps/ROOT.war


6️⃣ Started & Enabled Tomcat
```
systemctl start tomcat
systemctl enable tomcat


7️⃣ Verified the Deployment
Checked Tomcat status:
```
systemctl status tomcat

Verified application output:
```
curl http://localhost:6100


Output confirmed correct deployment:

Welcome to xFusionCorp Industries!


✅ Final Status: SUCCESS

Tomcat is correctly installed and running on port 6100, and the ROOT.war application is successfully deployed and accessible.
