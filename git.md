### git
> 查看两个节点的改动是否一致
> git add 命令
+ 这是个多功能命令，根据目标文件的状态不同，此命令的效果也不同：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等

###  公网配置gitlab ci cd 流程（使用原始的rpm安装方式,使用清华园repository）

+ install git policycoreutils-python gitlab gitlab-runner
+ + rpm -ivh gitlab-ce-10.8.1-ce.0.el6.x86_64.rpm
+ + + vim /etc/gitlab.rb  配置 url   
+ + + gitlab-ctl reconfigure  (gitlab-ctl start/status/stop)
+ + + gitlab 配置ssh-key https://www.cnblogs.com/hafiz/p/8146324.html
+ + rmp -ivh gitlab-runner-10.8.2-1.x86_64.rpm
+ + 配置runner 
+ + + 1 sudo gitlab-runner register 
+ + + 2 输入gitlab地址地址可以从项目中获取到
+ + + 3 输入token
+ + + 4 输入对Runner的描述，这个在GitLab’s UI可以修改，比如my-runner
+ + + 5 给Runner打个标签，这个在GitLab’s UI可以修改，比如java
+ + + 6 是否Runner执行没有标签的构建任务，输入true
+ + + 7 是否将Runner锁定到当前项目，这个在GitLab’s UI可以修改，输入true
+ + + 8 输入Runner的执行者，这里我选择shell
+ + 编写.gitlab-ci 脚本
+ + + 示例
```
image: maven:latest

# 本次构建的阶段：build package
stages:
  - build
  - package
# 构建 Job
build:
  stage: build
  tags:
    - java
  script:
    - echo "=============== 开始编译构建任务 ==============="
    - mvn compile
# 打包
package:
  stage: package
  tags:
    - java
  script:
    - echo "=============== 开始打包任务  ==============="
    - mvn package -Dmaven.test.skip=true
```
+ 相关控制命令
```
gitlab-ctl start 

systemctl start gitlab-runner  
systemctl status gitlab-runner

cat /etc/gitlab-runner/config.toml
gitlab-runner verify --delete --name xxx
sudo gitlab-runner restart
```

+ gitlab的备份和恢复
+ + https://www.cnblogs.com/root0/p/9268866.html

> git submodule 
```
git submodule add https://code.yidaoit.net/huajiemeifu/huawujie.git ydb-projects\huajiemeifu\huawujie
git submodule add https://code.yidaoit.net/heqianmo/backend.git ddb/heqianmo/backend
git clone https://code.yidaoit.net/yangtaimeng/workspace.git
git submodule init 
git submodule update 
```