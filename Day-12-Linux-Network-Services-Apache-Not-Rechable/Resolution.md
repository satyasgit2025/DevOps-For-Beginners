#1: Firstlry tried to check connectivity of apache service at port 6300 by using below commnad from jump host.
```
curl http://app01:6300
```
Output:
curl: (7) Failed to connect to stapp01 port 6300: No route to host

#2: SSH to App Server from jump host.
```
ssh user@app01
```
#3: Check Apache Service Status & Observed below error.
```
sudo systemctl status httpd
```
httpd.service was in failed state.
Error message indicated an address binding issue

Output:
(98)Address already in use: AH00072: make_sock: could not bind to address 0.0.0.0:6300
no listening sockets available, shutting down
AH00015: Unable to open logs

This indicated that port 6300 was already in use by another process.

#4: Check Port Usage
```
sudo netstat -tulnp | grep 6300
```
Output:
tcp 0 0 127.0.0.1:6300 0.0.0.0:* LISTEN 503/sendmail: accep

Port 6300 was already in use by sendmail.
Apache could not bind to port 6300 because of this conflict.

#5:Apache Configuration Validation
```
sudo grep -R "Listen" /etc/httpd -n
```
Output:
/etc/httpd/conf/httpd.conf:45:Listen 6300

Apache was correctly configured to listen on port 6300.
Main issue was port conflict, not configuration.

#6: Stop Conflicting Process
```
sudo netstat -tulnp | grep 6300
```
Output:
tcp 0 0 127.0.0.1:6300 0.0.0.0:* LISTEN 503/sendmail: accep

Kill the process:
```
sudo kill -9 503
```
Re-check:
```
sudo netstat -tulnp | grep 6300
```
no output means port 6300 is now free

#7: Start Apache
```
sudo systemctl start httpd
sudo systemctl status httpd
```
Check if Apache is now listening on port 6300:
```
sudo netstat -tulnp | grep 6300
```
Output:
tcp 0 0 0.0.0.0:6300 0.0.0.0:* LISTEN 1194/httpd
-Apache is now listening on 0.0.0.0:6300 (all interfaces), not just 127.0.0.1.

#8:Firewall / Network Layer Check in app server.
Checked firewalld was Not Installed
```
sudo firewall-cmd --list-all
```
Output:
sudo: firewall-cmd: command not found

This indicates firewalld is not installed or not being used.
The system is instead using iptables.

So need to Check iptables Rules
```
sudo iptables -L -n
```
Output:
Chain INPUT (policy ACCEPT)
target     prot opt source     destination
ACCEPT     all  --  0.0.0.0/0  0.0.0.0/0  state RELATED,ESTABLISHED
ACCEPT     icmp --  0.0.0.0/0  0.0.0.0/0
ACCEPT     all  --  0.0.0.0/0  0.0.0.0/0
ACCEPT     tcp  --  0.0.0.0/0  0.0.0.0/0  state NEW tcp dpt:22
REJECT     all  --  0.0.0.0/0  0.0.0.0/0  reject-with icmp-host-prohibited

There is a REJECT rule at the end of INPUT chain.
Port 6300 was not explicitly allowed, so traffic to port 6300 would be rejected.

Allowing Port 6300 via iptables
```
sudo iptables -I INPUT -p tcp --dport 6300 -j ACCEPT
```
Verify the updated rules:
``
sudo iptables -L -n
```
Output:
Chain INPUT (policy ACCEPT)
target     prot opt source     destination
ACCEPT     tcp  --  0.0.0.0/0  0.0.0.0/0  tcp dpt:6300
ACCEPT     all  --  0.0.0.0/0  0.0.0.0/0  state RELATED,ESTABLISHED
ACCEPT     icmp --  0.0.0.0/0  0.0.0.0/0
ACCEPT     all  --  0.0.0.0/0  0.0.0.0/0
ACCEPT     tcp  --  0.0.0.0/0  0.0.0.0/0  state NEW tcp dpt:22
REJECT     all  --  0.0.0.0/0  0.0.0.0/0  reject-with icmp-host-prohibited

#9: Verification
Local Verification on app server 1st.
```
curl http://localhost:6300
```
The default Apache HTTP Server Test Page was returned that onfirms Apache is serving content correctly on port 6300.

Remote Verification from Jump Host
```
curl http://app01:6300
```
The same Apache Test Page HTML was returned.

#10: Root Cause Summary
Port Conflict

sendmail was listening on 127.0.0.1:6300

Apache (httpd) failed to bind to port 6300, causing service failure

Network-Level Restriction

iptables INPUT chain had a final REJECT rule

No explicit rule was present to allow traffic on TCP port 6300

External access (from jump host) to port 6300 was rejected

#11: Key Commands Reference
# SSH to app server
ssh user@app01

# Check Apache status
sudo systemctl status httpd

# Start Apache
sudo systemctl start httpd

# Check Apache Listen directive
sudo grep -R "Listen" /etc/httpd -n

# Check port usage (with process)
sudo netstat -tulnp | grep 6300

# Kill conflicting process (example PID)
sudo kill -9 503

# Check firewall (firewalld - if available)
sudo firewall-cmd --list-all

# Check iptables rules
sudo iptables -L -n

# Allow port 6300 via iptables
sudo iptables -I INPUT -p tcp --dport 6300 -j ACCEPT

# Verify local access
curl http://localhost:6300

# Verify remote access from jump host
curl http://stapp01:6300

