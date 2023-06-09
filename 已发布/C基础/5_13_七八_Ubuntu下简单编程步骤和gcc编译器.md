# **七八_Ubuntu下简单编程步骤和gcc编译器**
# 一、Ubuntu下简单编程步骤
1. 创建一个.c文件

    ```
    touch hello.c
    ```

2. 进入C程序文件

    ```
    vi hello.c
    ```

3. 编写C语言程序

4. 保存编辑过的C文件

    ```
    :wq
    ```

5. 编译

      ```
      gcc hello.c
      ```

6. 执行程序 

   ```
   ./a.out
   ```


# 二、GCC 编译器
> gcc(GNU CCompiler)是GNU推出的功能强大，性能优越的多平台编译器，gcc编译器能将C,C++语言源程序编译连接成可执行文件。预处理、编译、汇编和链接。

## **2.1 预处理**

**gcc -E hello.c -o hello.i** 得到**预处理文件**，其中-E表示只进行预处理工作。

源文件在预处理阶段会被编译器生成.i文件，主要处理代码中以#开头的指令。如：对宏定义进行展开，以及展开头文件，同时还会去掉注释。

## **2.2 编译**

**gcc -S hello.i -o hello.s** 得到**汇编文件**，其中-S表示只生成汇编文件。

编译就是把预处理完的文件，进行语法分析、词法分析、语义分析及优化后生成相应的汇编代码文件，这个过程是整个程序构建的核心过程，也是最复杂的部分。

## **2.3 汇编**

**gcc -c hello.s -o hello.o** 其中-c表示的是只编译不链接。将汇编代码转变成机器可以执行的指令文件，也就是**目标文件**。

也可以直接使用：**gcc -c hello.c -o hello.o** 经过预处理、编译、汇编直接输出目标文件。

## **2.4 链接**

**gcc hello.o -o hello** 是将各种代码和数据片段收集并组合成一个**可执行文件**的过程，这个文件可以被加载到内存执行。