1:Installs pip3, which is needed to install Python packages.
```
yum install -y python3-pip
```
or
```
dnf install -y python3-pip
```
2:Removes any system package version of Ansible to avoid conflicts between yum-installed and pip-installed versions.

If no Ansible is installed via yum, you can skip this step.
```
dnf remove -y ansible
```
3:Ensures that pip and its dependencies are up-to-date; otherwise, older pip versions might fail to install Ansible or its dependencies.
```
pip3 install --upgrade pip setuptools wheel
```
4:Make sure pip3 is installed and confirm its path.
```
pip3 --version
which pip3
python3 -m pip --version

5:Install Ansible version 4.8.0 using pip3.
```
python3 -m pip install 'ansible==4.8.0' #This installs Ansible 4.8.0 globally (for all users) when run as root.
```
6:Pip installs binaries (like ansible, ansible-playbook) under /usr/local/bin, which might not be in $PATH for all users.

Creating a symlink to /usr/bin/ ensures that any user can run Ansible without modifying their $PATH.
```
ln -s /usr/local/bin/ansible /usr/bin/ansible
```
7:Create also symlink ansible-playbook.
```
ln -s /usr/local/bin/ansible-playbook /usr/bin/ansible-playbook
```
8: Check ansible version.
```
ansible --version
```

