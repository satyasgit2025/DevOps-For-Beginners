To disable direct SSH root login in any app server, we have to edit sshd_config file.

To achieve we can use below commands.

Make PermitRootLogin no at place of yes.
```
vi /etc/ssh/sshd_config
```
To See the changes
```
cat /etc/ssh/sshd_config
```
Restart the sshd
```
systemctl restart sshd
```

