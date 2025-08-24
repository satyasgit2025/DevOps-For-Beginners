Created user mariyam
```
[root@stapp03 ~]# useradd mariyam  
```
Set the expriry of mariyam's account
```
[root@stapp03 ~]# usermod -e 2024-04-15 mariyam  
```
To See the details mariyam's account detail
```
[root@stapp03 ~]# chage -l mariyam  
```
Last password change                                    : Aug 07, 2025

Password expires                                        : never

Password inactive                                       : never

Account expires                                         : Apr 15, 2024

Minimum number of days between password change          : 0

Maximum number of days between password change          : 99999

Number of days of warning before password expires       : 7

