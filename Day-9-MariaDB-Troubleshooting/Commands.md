1: To see the log
```
cat /var/log/mariadb/mariadb.log
```
# Got the below error 
"Job for mariadb.service failed because the control process exited with error code.
See "systemctl status mariadb.service" and "journalctl -xeu mariadb.service" for details".

2: As per error recomnadtion run below command.
```
systemctl status mariadb.service -l
```
# Got the below error
"systemd[1]: mariadb.service: Main process exited, code=exited, status=1/FAILURE"

3: To analyse log further again run.
```
cat /var/log/mariadb/mariadb.log
```
# Got the below error
"[ERROR] mariadbd: Can't create/write to file '/run/mariadb/mariadb.pid' (Errcode: 13 ""Permission denied"")
[ERROR] Can't start server: can't create PID file: Permission denied"

4: creates /run/mariadb in the runtime /run tmpfs so the server has a place for the PID file.
-p ensures the directory is created if it doesn’t exist (and won’t error if it already exists.
Note: will vanish after reboot.
```
mkdir -p /run/mariadb
```
5: Sets owner user = mysql and owner group = mysql.
Why: MariaDB normally runs as the mysql user (check User= in the systemd unit).
The process needs ownership (or write permissions) to create /run/mariadb/mariadb.pid.
If the directory were root-owned and not writable by the mysql user, creation fails.
```
chown mysql:mysql /run/mariadb
```
6: Sets permissions to rwxr-xr-x (owner: read/write/execute; group: read/execute; others: read/execute).
Why 755: owner (mysql) can create and write files; others can traverse the directory (needed for normal operation).
You could tighten to 750 if you want only owner+group access.
```
chmod 755 /run/mariadb
```
7: start the MariaDB servic3.
```
systemctl start mariadb

8: Permanent Fix Steps

To enable it for MariaDB add below
```
systemctl edit mariadb
```
RuntimeDirectory=mariadb
RuntimeDirectoryMode=0755

Reload systemd and restart MariaDB
```
systemctl daemon-reexec
systemctl restart mariadb
```

