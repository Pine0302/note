### macOS系统使用
> 1. 绑定/删除路由

+ sudo route -n add -net 192.168.189.0 -netmask 255.255.255.0 192.168.100.10  //添加ydb_vm 路由
+ sudo route -v delete -net 192.168.189.0 -gateway 192.168.100.10

+ sudo route -n add -net 192.168.188.0 -netmask 255.255.255.0 192.168.100.11  //添加coffe_vm 路由

> 2. 查找和清理大文件
+ 查看目录大小: sudo du -sh *
+ 进入大文件夹  查看文件: sudo du -d 1 -h
+ 然后对垃圾大文件进行删除 sudo rm -rf  big/

> 3. 安装 brew
+ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
+ 如果ssl连接不上 ping出raw.githubusercontent.com 的ip 然后在host里面加上 ip raw.githubusercontent.com
+ 上述安装方式需要开启vpn 暂时替代方法 /bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"

> 4. 查看端口占用 
+ sudo lsof -i:1080   kill -9 pid

  
>5. ssr 命令
```
 ./ssr_mac_client/ssr-client -c ./ssr_mac_client/config.json 
/Users/pine/Desktop/ssr/ssr_mac_client/daemon/daemon_ssr.sh 
 ```

 > 6. 安装IPROUTE2 
 ```
 brew install iproute2mac
```
