@[TOC](目录)

# 一、查询文件信息
## 1、stat
int stat(const char *path, struct stat *buf);
- 功能：获取文件属性
- 参数
	- path：文件路径名
	- buf：保存文件属性信息的结构体
- 返回值
	- 成功：0
	- 失败：-1

结构体
```c
	struct stat {
        dev_t     st_dev;         /* 设备包含文件ID */
        ino_t     st_ino;     /* inode号 */
        mode_t    st_mode;    /* 文件类型和权限 */
        nlink_t   st_nlink;   /* 硬链接数 */
        uid_t     st_uid;     /* 用户ID */
        gid_t     st_gid;     /* 组ID */
        off_t     st_size;    /* 大小 */
        dev_t     st_rdev;        /* 设备ID */
        time_t    st_atime;   /* 最后访问时间 */
        time_t    st_mtime;   /* 最后修改时间 */
        time_t    st_ctime;  /* 最后状态改变时间 */
};
```
练习
**实现ls -l filename**
(1) 获取文件属性
- 方法一
![在这里插入图片描述](https://img-blog.csdnimg.cn/05c2a451fa3e49e98e3a130cc193471e.png)

(2) 获取文件权限
- 方法一
![在这里插入图片描述](https://img-blog.csdnimg.cn/f76af6c4471a4bfc986034147b58146b.png)

- 方法二
![在这里插入图片描述](https://img-blog.csdnimg.cn/4207e90e3c1f4fa1b12d685c34ed4cd2.png)

(3) 获取文件链接数
![在这里插入图片描述](https://img-blog.csdnimg.cn/3ec2f47685e04511b3ca337eaf3bb63b.png)

(4) 获取用户名和组名
![在这里插入图片描述](https://img-blog.csdnimg.cn/b127e0e84bbb460096c1400809cf76f4.png)

(5) 获取文件大小
![在这里插入图片描述](https://img-blog.csdnimg.cn/28aa8d8939be40d9934921b42d569038.png)

(6) 获取文件上次修改时间
![在这里插入图片描述](https://img-blog.csdnimg.cn/18668d86bc5b46b1bae1e75a2e0e12c6.png)
其中，st.st_mtime返回一个结构体，ctime返回时间的字符串
%.12s表示显示12位字符串

## 2、stat fstat lstat区别
![在这里插入图片描述](https://img-blog.csdnimg.cn/a576c4c1ae904768af79f0152ee54ea1.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/23b3d61229834eefaf59d487c46eed35.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/a85297f2c75c4b96b7058c1e778fb6ff.png)

# 二、目录操作
## 2.1 opendir
DIR *opendir(const char *name);
- 功能：获得目录流
- 参数：要打开的目录
- 返回值：
	- 成功：目录流
	- 失败：NULL
## 2.2 readdir 
struct dirent *readdir(DIR *dirp);
- 功能：读目录
- 参数：要读的目录流
- 返回值：
	- 成功：读到的信息
	- 失败：NULL，设置errno号

> 返回值为结构体，该结构体成员为描述该目录下的文件信息

dirent结构体
```c
struct dirent {
        ino_t   d_ino;                   /* 索引节点号*/
        off_t   d_off;               /*在目录文件中的偏移*/
        unsigned short d_reclen;    /* 文件名长度*/
        unsigned char  d_type;      /* 文件类型 */
        char    d_name[256];      /* 文件名 */
};
```
## 2.3 closedir
int closedir(DIR *dirp);
- 功能：关闭目录
- 参数：dirp：目录流
## 例
![在这里插入图片描述](https://img-blog.csdnimg.cn/8ed4d311251641eda759cc9649b2cfd1.png)
## 练习：实现ls操作
![在这里插入图片描述](https://img-blog.csdnimg.cn/9788a2d84b294b6fae5b79c6a9042047.png)

# 三、库
## 3.1 库的定义
当使用别人的函数时除了包含头文件以外还要有库
- 头文件：函数声明、结构体等类型定义、头文件、宏定义
- 库：就是把一些常用函数的目标文件打包在一起，提供相应函数的接口，便于程序员使用；本质上来说库是一种可执行代码的二进制形式

由于windows和linux的本质不同，因此二者库的二进制是不兼容的

## 3.2 库的分类
### 3.2.1 静态库
静态库在程序编译时会被连接到目标代码中。

优缺点
- **优点：**
程序运行时将不再需要该静态库；
运行时无需加载库，运行速度更快
- **缺点：**
静态库中的代码复制到了程序中，因此体积较大；
静态库升级后，程序需要重新编译链接
### 3.2.2 动态库
动态库是在程序运行时才被载入代码中
优缺点
- 优点：
程序在执行时加载动态库，代码体积小；
程序升级更简单；
不同应用程序如果调用相同的库，那么在内存里
只需要有一份该共享库的实例。
- 缺点：
运行时还需要动态库的存在，移植性较差
静态库和动态库，本质区别是代码被载入时刻不同。

## 3.3 创建库
### 3.3.1 静态库制作

 1. 将源文件编译生成目标文件：
 gcc -c xxx.c -o xxx.o
 2. 创建静态库用ar命令，它将很多.o转换成.a：
 ar crs libxxx.a xxx.o
 3. 测试使用静态库
 gcc xxx.c -L指定库的路径 -l指定库名
 
例
![在这里插入图片描述](https://img-blog.csdnimg.cn/f3a3acad0444411cb6b487c169a366af.png)
### 3.3.2 动态库制作
 1. 我们用gcc来创建共享库（两条命令）
gcc -fPIC -c xxx.c -o xxx.o
gcc -shared  -o libxxx.so xxx.o
2. 测试动态库使用
gcc xxx.c -L路径 -l库名
![在这里插入图片描述](https://img-blog.csdnimg.cn/87072bdb6ac14f0b85876fb53f79529a.png)
👉上面错误的解决方法
- (1) 把库拷贝到/usr/lib和/lib目录下。(此方法编译时不需要指定库的路径) 
- (2) 在LD_LIBRARY_PATH环境变量中加上库所在路径。 
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:. 
（终端关闭，环境变量就没在了）
![在这里插入图片描述](https://img-blog.csdnimg.cn/04fed7a6e26b483a9728917c9ecf454d.png)

- (3) 添加/etc/ld.so.conf.d/*.conf文件。把库所在的路径加到文件末尾，并执行ldconfig刷新
sudo vi /etc/ld.so.conf.d/*.conf
![在这里插入图片描述](https://img-blog.csdnimg.cn/3f57c98d59c8489596b3d84ee579e634.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/4398ba16eb7541668ba8672de2f8bf45.png)

