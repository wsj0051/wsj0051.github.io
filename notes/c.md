# C语言学习
## Hello World
新建hello.c文件，输入以下内容
```c
#include <stdio.h>
int main(void)
{
    printf("Hello World\n");
    return 0;
}
```
```
gcc -o hello.cgi hello.c
```
## System系统函数：
+ 使用system函数可以调用其他程序
  - 需要使用系统库：`<stdlib.h>`
  - 可以用在Linux平台和windows平台，但是调用的命令行不同
  
## C语言编译过程
1. 预处理 `gcc -E a.c -o a.i`
   - 宏定义展开
   - 头文件展开
   - 删除注释
   - 条件编译
2. 编译 `gcc -S a.i -o a.s`
   - 检查语法
   - 转化成汇编语言
3. 汇编 `gcc -c a.s -o a.o`
   - 将汇编语言转化为机器语言
4. 链接 `gcc a.o -o a.exe`
   - 将库文件链接变成可执行文件

## 汇编语言
1. 新建项目创建文件
2. 写c语言源代码添加断点，调试执行
3. 程序会停止在断点处，在调试菜单栏中选择窗口，在列表中选择反汇编，查看汇编源代码 
    ```c
    //汇编代码
        __asm
        {
                mov a, 3
                mov b, 4
                mov eax, a
                add eax, b
                mov c, eax
        }
    ```
## 处理C语言函数的警告：
1. 放在程序第一行
    ```c
    #define _CRT_SECURE_NO_WARNINGS
    ```
2. 任意位置`#pragma warning(disable:4996`
3. 项目右击选择属性，在打开对话框中选择C/C++处理器，在预处理器定义中编辑 `_CRT_SECURE_NO_WARNINGS`
4. linux下没有该错误