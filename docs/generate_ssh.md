https://www.hostinger.com/tutorials/vps/how-to-generate-ssh-keys-on-putty  

membuat user dengan hak akses sudoers

Ubuntu  

```
adduser username
usermod -aG sudo username
```

CentOS

```
adduser username
passwd username
usermod -aG wheel username
```

```
mkdir -p ~/.ssh | touch ~/.ssh/authorized_keys  
chmod 0700 ~/.ssh; chmod 0644 ~/.ssh/authorized_keys  
nano ~/.ssh/authorized_keys (kopi public key)  
```
