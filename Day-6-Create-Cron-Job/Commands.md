Install cronie package (cron daemon package)
```
yum install -y cronie
```
Enable and start the crond service & ensure the service is active and running.
```
systemctl enable crond
systemctl start crond
systemctl status crond
```
Add a cron job for the root user
```
crontab -e
```
Add the cron, Save and exit the editor. 
```
*/5 * * * * echo hello > /tmp/cron_text
```
Verify the cron entry
```
crontab -l
```
Verification After 5 Minutes
```
cat /tmp/cron_text
```
It will show hello
