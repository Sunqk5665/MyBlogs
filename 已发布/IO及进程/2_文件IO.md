## 文件IO

## 一、概念⭐⭐

- 在**系统中**定义的一组用于输入输出的函数接口（注意和标准IO概念上的区别🚨👉标准IO是在**C库中**定义的一组用于输入输出的函数接口）

## 二、特点⭐⭐⭐

- 1）**没有缓冲机制**，每次调用都会引起系统调用
- 2）围绕**文件描述符**进行操作，文件描述符是**非负整数（>=0）**，**依次分配**
- 3）**默认打开三个文件描述符**，**0（标准输入），1（标准输出），2（标准错误）**
- 4）**可以操作任意类型文件，目录文件除外**

## 三、函数⭐⭐⭐⭐

### 3.1 打开文件 open

- **int open(const char *pathname, int flags);**
- 功能：打开文件
- 参数：

	- pathname：文件名或包含路径的文件名
	- flags：打开文件的方式

		- O_RDONLY：只读
		- O_WRONLY:只写
		- O_RDWR：可读可写
		- O_CREAT:创建
		- O_TRUNC：清空
		- O_APPEND：追加 

- 返回值

	- 成功：文件描述符
	- 失败：-1

- 当第二个参数中有O_CREAT选项时，需要给open函数传递第三个参数，指定创建文件的权限 
- **int open(const char *pathname, int flags, mode_t mode);
创建出来的文件权限为指定权限值&(~umask)  //umask为文件权限掩码**
- 标准IO文件打开权限与
文件IO文件打开权限对比

	- [![pC9TAMR.png](https://s1.ax1x.com/2023/06/04/pC9TAMR.png)](https://imgse.com/i/pC9TAMR)

### 3.2  关闭文件 close

- **int close(int fd);**
- 功能：关闭文件
- 参数：fd：文件描述符
- 返回值：

	- 成功：0
	- 失败：-1

### 3.3 读写操作

- read

  - **ssize_t read(int fd, void *buf, size_t count);**
  - 功能：从一个已打开的可读文件中读取数据
  - 参数

  	- fd  文件描述符
  	- buf  输入缓冲区指针
  	- count  要读入的字节数

  - 返回值

  	- 成功：实际读到的字节数
  	- 返回0：表示读到文件结尾	
  	- 返回-1：表示出错,并设置errno号

- write

  - **ssize_t write(int fd, const void *buf, size_t count);**
  - 功能：向指定文件描述符中，写入 count个字节的数据。
  - 参数

  	- fd   文件描述符
  	- buf   要写的内容
  	- count  要写入的字节数

  - 返回值

  	- 成功：实际写入的字节数
  	- 失败 ： -1
  	- 没有数据要写：0

- 例

  - [![pC9TEs1.md.png](https://s1.ax1x.com/2023/06/04/pC9TEs1.md.png)](https://imgse.com/i/pC9TEs1)

- 练习：实现Linux下的cp命令

  - [![pC9TeZ6.png](https://s1.ax1x.com/2023/06/04/pC9TeZ6.png)](https://imgse.com/i/pC9TeZ6)

### 3.4 定位操作 lseek

- **off_t lseek(int fd, off_t offset, int whence);**
- 功能：设定文件的偏移位置	
- 参数

	- fd：文件描述符
	- offset偏移量

		- 正数：向文件结尾位置移动
		- 负数：向文件开始位置

	- whence  相对位置

		- SEEK_SET   开始位置
		- SEEK_CUR   当前位置
		- SEEK_END   结尾位置

- 返回值

	- 成功：文件的当前位置
	- 失败：-1

## 四、文件IO与标准IO的对比

​	[![pC9TgoT.png](https://s1.ax1x.com/2023/06/04/pC9TgoT.png)](https://imgse.com/i/pC9TgoT)