✅ 1. Install iptables on EACH App Server
```
sudo yum install -y iptables-services
sudo systemctl enable iptables
sudo systemctl start iptables
```
This installs iptables + dependencies and enables it on boot.

✅ 2. Block port 6400 for everyone except LBR host

Run on each app server:

Assume LB ip 172.16.238.14 & allow to access port 6400 in ip tables
```
sudo iptables -A INPUT -p tcp --dport 6400 -s 172.16.238.14 -j ACCEPT
```
Block all other IPs from port 6400.
```
sudo iptables -A INPUT -p tcp --dport 6400 -j DROP
```
⚠ IMPORTANT — Rule Order Must Be Correct

ACCEPT must appear before DROP.

Check:
``
sudo iptables -L INPUT -n --line-numbers
```
You should see:

1  ACCEPT  tcp  --  172.16.238.14   0.0.0.0/0   tcp dpt:6400
2  DROP    tcp  --  0.0.0.0/0       0.0.0.0/0   tcp dpt:6400

✅ 3. Make the firewall rules persistent
Save the rules:
``
sudo service iptables save
```
OR (preferred):
```
sudo iptables-save | sudo tee /etc/sysconfig/iptables
```
Verify persistence file:
```
cat /etc/sysconfig/iptables
```
Check for these entries:

-A INPUT -p tcp -s 172.16.238.14 --dport 6400 -j ACCEPT
-A INPUT -p tcp --dport 6400 -j DROP

🔄 4. (Optional) Reboot to confirm rules survive reboot
```
sudo reboot
```
After restart:
```
sudo iptables -L INPUT -n
```
