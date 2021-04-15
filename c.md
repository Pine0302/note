+ c 语言代码变成可执行文件的过程
```
gcc -E main.c -o main.i //第一步：hello.c（文本）经过预编译生成hello.i（文本）
gcc -S main.i -o main.S //第二步：hello.i（文本）经过编译器生成hello.s（汇编。文本）
gcc -c main.c -o main.o //第三步：hello.s（文本）经过汇编器生成hello.o（二进制）。
gcc main.o -o main    //第四步：hello.o（二进制）经过链接器生成hello可执行文件。
```
+ 反汇编指令
```
objdump -d -S gdbtest
objdump -d -S gdbtest.o
```
