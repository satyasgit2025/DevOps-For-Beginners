1:Let's Assume that banner user is on app server 3, so using below command connect server3.
```
ssh banner@appserver3ip
```
2: As per note zip package must be installed on given App Server before executing the script.

This package is essential for creating the zip archive of the website files. Install it manually outside the script.
```
yum install -y zip
```
3:Create a zip archive named xfusioncorp_ecommerce.zip of /var/www/html/ecommerce directory & Save the archive in /backup/ on App Server 3 by using below command.
```
zip -r /backup/xfusioncorp_ecommerce.zip /var/www/html/ecommerce
```
4:Now we need to Copy the created archive to Nautilus Backup Server server in /backup/ location & make sure script won't ask for password while copying the archive file.

Setup passwordless SSH from App Server 3 → Backup Server

For this case we need to ensure that user banner should be on both server if not then create the user banner on backup server too.

Login the backup server with repective credentials & check user banner is there or not by using below command.
```
id banner
```
or 
```
cat /etc/passwd  # check the banner in list.
```
if user is not there then create user
```
useradd banner
```
Set passwd
```
passwd banner
```

Again login app server3 by user banner & generate shh key by using below command
```
ssh-keygen -t rsa -b 4096
```
Add same key to backup server by banner user & give the passwd which earlier set in backup server for user banner.
```
ssh-copy-id banner@stbkp01.stratos.xfusioncorp.com   # backup server ip or host
```
4: Login the app server 3 & prepare bash script in /scripts folder
```
cd /scripts
```
```
vi ecommerce_backup.sh  # paste below script press esc>>shift+:>>wq!>>enter
```

#!/bin/bash

# Variables
SRC_DIR="/var/www/html/ecommerce"
ARCHIVE_NAME="xfusioncorp_ecommerce.zip"
LOCAL_BACKUP="/backup/${ARCHIVE_NAME}"
REMOTE_USER="banner"
REMOTE_HOST="stbkp01.stratos.xfusioncorp.com"
REMOTE_BACKUP="/backup/"

# Step 1: Create zip archive
zip -r "$LOCAL_BACKUP" "$SRC_DIR" >/dev/null 2>&1

# Step 2: Copy archive to Nautilus Backup Server
scp "$LOCAL_BACKUP" ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_BACKUP}

5: Make Script executable by using below command & run the script.
```
chmod +x /scripts/ecommerce_backup.sh
```
```
/scripts/ecommerce_backup.sh
```
o/p willbe like 
[banner@stapp03 scripts]$ ./ecommerce_backup.sh
xfusioncorp_ecommerce.zip                                                                                     100%  623     1.0MB/s   00:00    i

6: Automate the Backup with Cron (Optional)

Edit the crontab for the banner user on App Server 3 (Do not use sudo inside the script).
```
crontab -e
```
Add a cron job to run the script every day at 2 AM:
```
0 2 * * * /scripts/ecommerce_backup.sh
```






