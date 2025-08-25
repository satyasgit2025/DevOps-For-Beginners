Access the jump server iwth user thor & generate rsa key.
```
ssh thor@172.16.240.10
```
Generate a new SSH key pair using the RSA algorithm with a 4096-bit key size.
```
ssh-keygen -t rsa -b 4096
```
Copy the public key to each app server’s sudo user.
```
ssh-copy-id satya@172.16.240.11
ssh-copy-id shiva@172.16.240.12
```
Test passwordless login (connect without being asked for a password).
```
ssh satya@172.16.240.11
ssh shiva@172.16.240.12
```
