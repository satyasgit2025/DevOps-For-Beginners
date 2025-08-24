Task Can be divided in four steps to understand easily.

1: Install required SELinux packages.

Make sure all necessary SELinux components are installed so configuration can be done later.
```
yum install selinux-policy selinux-policy-targeted policycoreutils
```
2: Back up the SELinux config before changing it.
```
cp -av /etc/selinux/config /etc/selinux/config.bak.$(date +%F-%H%M%S)
```

3: Permanently disable SELinux for now.

SELinux will be re-enabled after required configuration changes are applied.
```
vi /etc/selinux/config
```
Do SELINUX=disabled after hitting vi command else use below cmd (Before it should be SELINUX=enforcing).
```
sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config
```

4: Do NOT reboot the server immediately.

There’s already a scheduled maintenance reboot, so no manual reboot is needed now.


5: Ignore the current runtime SELinux status.

After reboot, SELinux should be disabled as per configuration done now.

Note: SELinux will still appear enabled (or permissive) until the scheduled reboot. After the reboot, it will be disabled.
