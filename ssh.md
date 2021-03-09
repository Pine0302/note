### ssh command
从服务器A免密登录服务器B
在A服务器上:
```
cd root
ssh-keygen   ...
cd .ssh
```
vi .ssh/config
```
Host            hqm
HostName        60.205.186.31
Port            22
User            root
IdentityFile    ~/.ssh/id_rsa
```

在B服务器上
把服务器A上面的id_rsa.pub append 到/root/.ssh/authorized_keys文件下面


