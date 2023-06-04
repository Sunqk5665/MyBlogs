## 标准I/O

## 一、概念

- **在C库中定义的一组用于输入输出的函数接口**

## 二、特点⭐⭐⭐

- 1）通过**缓冲机制**减少系统调用，提高效率
- 2）围绕**流**进行操作，流用**FILE ***表示。
- 3）标准IO**默认打开了三个流**，分别是**标准输入stdin，标准输出stdout，标准错误stderr**

## 三、缓冲区⭐⭐⭐

### 3.1 全缓冲

- 针对文件
- 刷新条件

	- 程序正常结束
	- 缓冲区满刷新
	- 强制刷新：fflush

### 3.1 行缓冲

- 针对终端
- 刷新条件

  - \n

    - [![pC9K26g.png](https://s1.ax1x.com/2023/06/03/pC9K26g.png)](https://imgse.com/i/pC9K26g)
      - 若不加'\n'，缓冲区未满，运行程序不会有任何输出

  - 程序正常结束
  - 缓冲区满刷新

    - 方法一：循环往缓冲区中放入固定大小的数据，直到达到行缓冲满，看一共存入了几个固定大小的数据，从而可算出行缓冲的大小。

      - [![pC9Kfmj.md.png](https://s1.ax1x.com/2023/06/03/pC9Kfmj.md.png)](https://imgse.com/i/pC9Kfmj)

    - 方法二：使用标准输出stdout的
      成员\_IO_buf_end和\_IO_buf_base
      - [![pC9KThV.md.png](https://s1.ax1x.com/2023/06/03/pC9KThV.md.png)](https://imgse.com/i/pC9KThV)
        - 未开辟缓冲区，故缓冲区大小为0

      - [![pC9Kb1U.md.png](https://s1.ax1x.com/2023/06/03/pC9Kb1U.md.png)](https://imgse.com/i/pC9Kb1U)
        - 第一个printf随便往缓冲区中放入数据来开辟出一个缓冲区，这样可求得缓冲区大小为1024Byte

  - **强制刷新：fflush**

    - [![pC9KXnJ.md.png](https://s1.ax1x.com/2023/06/03/pC9KXnJ.md.png)](https://imgse.com/i/pC9KXnJ)
    - [![pC9KzA1.md.png](https://s1.ax1x.com/2023/06/03/pC9KzA1.md.png)](https://imgse.com/i/pC9KzA1)

### 3.3 不缓冲

- 没缓冲区，标准错误

## 四、函数接口⭐⭐⭐⭐

### 4.1 打开

#### 4.1.1 fopen

- **FILE *fopen(const char *path, const char *mode)**
- 功能

	- 打开文件

- 参数

	- path：打开的文件
	- mode：打开的方式

		- r/rb：只读，当文件不存在时报错，文件流定位到文件开头
		- r+/r+b：可读可写，当文件不存在时报错，文件流定位到文件开头
		- w/wb：只写，文件不存在创建，存在清空
		- w+/w+b：可读可写，文件不存在创建，存在清空
		- a/ab：追加(在末尾写)，文件不存在创建，存在追加，文件流定位到文件末尾
		- a+/a+b：读和追加，文件不存在创建，存在追加，🚨📢👉读文件流定位到文件开头，写文件流定位到文件末尾

- 返回值

	- 成功：文件流指针
	- 失败：NULL，并且会设置错误码

- 例

	- [![pC9MStx.png](https://s1.ax1x.com/2023/06/03/pC9MStx.png)](https://imgse.com/i/pC9MStx)

#### 4.1.2 freopen

- **FILE * freopen(const char *pathname,  const char *mode,  FILE* fp)**
- 功能：将指定的文件流重定向到打开的文件中
- 参数

	- path：文件路径
	- mode：打开文件的方式（同fopen）
	- fp：文件流指针

- 返回值

	- 成功：返回文件流指针
	- 失败：NULL

- [![pC9MK9f.md.png](https://s1.ax1x.com/2023/06/03/pC9MK9f.md.png)](https://imgse.com/i/pC9MK9f)
  - /dev/tty是一个特殊文件，代表当前正在使用的终端设备

#### 4.1.2 容错机制perror

- perror是一个库函数，这个函数内部会自己获取errno的值，调用这个函数会直接把错误提示字符串打印出来。此外，我们也可以在错误提示字符串前添加一些自己想要打印的信息→perror("fopen err");
- man手册信息

	- [![pC9MM38.md.png](https://s1.ax1x.com/2023/06/03/pC9MM38.md.png)](https://imgse.com/i/pC9MM38)

### 4.2 关闭

### 4.2.1 fclose

- **int fclose(FILE* stream);**
- 功能

	- 关闭文件

- 参数

	- stream：文件流

- 返回值

	- 成功

		- 返回0

	- 失败

		- 返回EOF

### 4.3 读写操作

#### 4.3.1 字符I/O

- fgetc

  - **int  fgetc(FILE * stream)**
  - 功能：从文件中读取一个字符
  - 参数：stream：文件流
  - 返回值：

  	- 成功：整型，即读到的字符的ASCII码值
  	- 文件结束或出现错误返回EOF

- fputc

  - **int fputc(int c, FILE * stream)**
  - 功能：向文件流stream中写入一个字符c
  - 参数

  	- c：要写的字符的ASCII码值，或者直接写 '字符'
  	- stream：文件流

  - 返回值

  	- 成功：写的字符的ASCII码值
  	- 失败：EOF

- 练习1

  - [![pC9Mljg.png](https://s1.ax1x.com/2023/06/03/pC9Mljg.png)](https://imgse.com/i/pC9Mljg)

    - 文件以只读方式打开

      [![pC9H0rn.png](https://s1.ax1x.com/2023/06/04/pC9H0rn.png)](https://imgse.com/i/pC9H0rn)

  - [![pC9MGHs.png](https://s1.ax1x.com/2023/06/03/pC9MGHs.png)](https://imgse.com/i/pC9MGHs)

    - [![pC9HQKA.png](https://s1.ax1x.com/2023/06/04/pC9HQKA.png)](https://imgse.com/i/pC9HQKA)

- 练习2

  - 通过fgetc实现cat功能
  - [![pC9MN40.md.png](https://s1.ax1x.com/2023/06/03/pC9MN40.md.png)](https://imgse.com/i/pC9MN40)
    - 注意文件末尾的判断

#### 4.3.2 行I/O

- fgets

  - **char * fgets(char *s,  int size,  FILE * stream);**
  - 功能：从文件流中每次读取一行字符串至s所指向的字符串中
  - 参数

  	- s：存放字符串的地址
  	- size：一次读取的字符串s的大小
  	- stream：文件流指针

  - 返回值

  	- 成功：s的地址
  	- 失败或读到文件末尾：NULL

  - 特性

  	- 该函数从流中连续读字符直至读到换行符'\n'或者读够size-1个字符（包括换行符）为止。所读入的这一行字符，包括最后的换行符，存储在s指定的字符串中，并且在其末尾添加一个空字符'\0'作为该字符串的结束标志。
  如果要读入的这一行，包括结尾的换行符，长度大于size-1，则只有部分字符（size-1个）被读入。下次调用将返回此行剩余部分。

- fputs

  - **int  fputs(const char *s,  FILE * stream);**
  - 功能：把以空字符(\0)结尾的字符串输出到指定文件流中，末尾的空字符(\0)并不输出。
  由于字符串并没有要求一定换行符为结尾，所以这个函数也不一定是一次输出一行的。
  - 参数

  	- s：要写的内容
  	- stream：文件流

  - 返回值

  	- 成功：非负整数
  	- 失败：EOF

- 练习

  - 通过fgets实现"wc -l 文件名"命令功能（计算文件行数）

    - [![pC9MaCV.md.png](https://s1.ax1x.com/2023/06/03/pC9MaCV.md.png)](https://imgse.com/i/pC9MaCV)

  - 实现cp功能

    - [![pC9Md3T.png](https://s1.ax1x.com/2023/06/03/pC9Md3T.png)](https://imgse.com/i/pC9Md3T)
    - diff命令

      - 比较文件差异，若两个文件相同，则没有输出，若不同，则会有相应的提示

  - 实现head -n 文件名 
     ./a.out -3 name

     - [![pC9Msb9.png](https://s1.ax1x.com/2023/06/03/pC9Msb9.png)](https://imgse.com/i/pC9Msb9)

#### 4.3.3 块I/O

- fread

	- **size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);**
	- 功能：从stream文件流读取nmenb个数据项存放至ptr所指的数组中，其中每一项数据长度为size大小，故所读取总字节数为size*nmemb
	- 参数

		- ptr ：用来存放读取元素
		- size ：元素大小，用sizeof(数据类型)表示
		- nmemb ：读取元素的个数
		- stream ：要读取的文件流指针

	- 返回值

		- 成功：读取的元素的个数
		- 读到文件尾： 0
		- 失败： -1

- fwrite

	- **size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream);**
	- 功能：从ptr所指的数组中写出nmemb个数据项至stream指定的流
	- 参数

		- 和fread一样

	- 返回值

		- 成功：写的元素个数
		- 失败 ：-1

- 例

	- [![pC9oQuq.md.png](https://s1.ax1x.com/2023/06/04/pC9oQuq.md.png)](https://imgse.com/i/pC9oQuq)

- 练习：实现cp fread fwrite 复制一张图片

	- [![pC9o1bV.md.png](https://s1.ax1x.com/2023/06/04/pC9o1bV.md.png)](https://imgse.com/i/pC9o1bV)
	- 注意点：
1、命令行传参要进行容错判断，可以判断传入的参数的数量argc
2、图片文件是以字节为单位的二进制数据组成的，所以用fread和fwrite进行操作，而且作为中转的数组的数据类型应该为char类型，因为这样，在通过fread和fwrite循环读写时才能确保源文件中恰好最后一个字节的二进制文件复制到目标文件中；如果作为中转的数组的数据类型为int型，则无法保证上述效果。

### 4.4 定位操作

- rewind

	- **void rewind(FILE *stream);**
	- 功能：将文件位置指针定位到起始位置

- fseek

	- **int fseek(FILE *stream, long offset, int whence);**
	- 功能：文件的定位操作
	- 参数

		- stream：文件流
		- offset：偏移量：正数表示向后文件尾部偏移，负数表示向文件开头偏移
		- whence：相对位置：

			- SEEK_SET:相对于文件开头
			- SEEK_CUR:相对于文件当前位置
			- SEEK_END:相对于文件末尾

		- 返回值

			- 成功：0
			- 失败：-1

		- 📢注：当打开文件的方式为a或a+时，fseek不起作用 

- ftell

	- **long ftell(FILE *stream);**
	- 功能：获取当前的文件位置
	- 参数：要检测的文件流
	- 返回值

		- 成功：当前的文件位置（从文件开始到当前文件流的字节数）
		- 出错：-1

- 获取文件长度

	- [![pC9oJ5F.png](https://s1.ax1x.com/2023/06/04/pC9oJ5F.png)](https://imgse.com/i/pC9oJ5F)

- 例

	- [![pC9oN8J.png](https://s1.ax1x.com/2023/06/04/pC9oN8J.png)](https://imgse.com/i/pC9oN8J)

### 4.5 文件结束和错误

- [![pC9oavR.md.png](https://s1.ax1x.com/2023/06/04/pC9oavR.md.png)](https://imgse.com/i/pC9oavR)
- feof

  - **int  feof(FILE * stream);**
  - 功能：判断文件有没有到结尾
  - 返回：到达文件末尾，文件结束条件指示器被设置，返回非零值

- ferror

  - **int ferror(FILE * stream);**
  - 功能:检测文件有没有出错
  - 返回：文件出错，错误指示器被设置，返回非零值

- 例

  - [![pC9oBb6.png](https://s1.ax1x.com/2023/06/04/pC9oBb6.png)](https://imgse.com/i/pC9oBb6)