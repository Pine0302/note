+ 环境准备
vagrant plugin install vagrant-librarian-chef --plugin-clean-sources --plugin-source https://rubygems.org

+ 为了把C语言源程序编译成IA-32机器指令，X86-64位计算机系统需要先运行以下命令
sudo apt-get install build-essential module-assistant
sudo apt-get install gcc-multilib g++-multilib
gcc -g -m32  gdbtest.c -o gdbtest  //编译成32位命令

+ 模糊匹配 -> 输入 --> 计算行数
find . -name "*.c" -o -name "*.h" | xargs cat | wc -l

+ 找目录中中main函数
grep -n main $(find . -name "*.c")

find . -name "*.c" | xargs grep --color -nse '\<main\>'

+ 可以搜索+ 编辑
vim $(fzf) 

+ vim 函数跳转
```
//安装
sudo apt-get install exuberant-ctags
ctags -R .
//使用 
ctrl+]  ctrl+t
//出现No tags file提示如何解决！
//打开.vimrc配置文件设置下：
//设置vim搜索tags的逻辑，该目录开始往上搜索
set tags=./tags,./TAGS,tags;~,TAGS;~
```

