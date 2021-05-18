+ 环境准备
vagrant plugin install vagrant-librarian-chef --plugin-clean-sources --plugin-source https://rubygems.org

+ 为了把C语言源程序编译成IA-32机器指令，X86-64位计算机系统需要先运行以下命令
sudo apt-get install build-essential module-assistant
sudo apt-get install gcc-multilib g++-multilib
gcc -g -m32  gdbtest.c -o gdbtest  //编译成32位命令