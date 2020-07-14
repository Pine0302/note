### macOS系统使用
> 1.绑定路由
+ sudo route -n add -net 192.168.189.0 -netmask 255.255.255.0 192.168.100.10

> 2.查找和清理大文件
+ 查看目录大小: sudo du -sh *
+ 进入大文件夹  查看文件: sudo du -d 1 -h
+ 然后对垃圾大文件进行删除 sudo rm -rf  big/

> 3.安装 brew
+ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
+ 如果ssl连接不上 ping出raw.githubusercontent.com 的ip 然后在host里面加上 ip raw.githubusercontent.com