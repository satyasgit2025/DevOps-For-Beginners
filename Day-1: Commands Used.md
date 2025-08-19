Create a user who cannot log into shell (Non-Interactive Shell).
  Let's assume that user is satya, below commands will be used to meet the requirement.
    Access the required server go at root user by using command sudo -i
  [root@server ~]$ useradd satya
"[root@server  ~]$ getent passwd satya
satya:x:1002:1002::/home/satya:/bin/bash
[root@server ~]$ usermod -s /sbin/nologin satya
[root@server ~]$ getent passwd satya
satya:x:1002:1002::/home/satya:/sbin/nologin"  (nologin means user can't access shell)
"[root@server ~]$ ls -ld /home/satya/
drwx------ 2 satya satya 4096 Aug  7 07:50 /home/satya/"

  
