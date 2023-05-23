> ubantu作为一款非常好用的Linux发行版本，深受广大开发者的喜爱😃，为了开发的方便，人们常常在windows电脑中安装VMware虚拟机来运行Linux系统，我们时常会遇到这样一种情况：无法互传虚拟机与主机文件。
> 原因就是没有安装或没有在Ubuntu中正确安装VMwareTools工具
> 接下来就来进行VMwareTools工具的安装👇

⚠️前面部分应该算是我踩的坑，若想快速实现你想要的功能，可滑到到文末，找到 **注意！！！** 和 **新的解决方法：👇**标题，这两个标题下面的文字便是新方式

在进入虚拟机并打开ubuntu后，点击“虚拟机”→点击“安装VMware tools”（若之前点击过这个选项但没有进行安装VMware tools，则显示“更新VMware tools”，我这里就是😄）

[![p9Mx7iF.png](https://img-blog.csdnimg.cn/img_convert/58b74f1a59a0e88d23064f3fba9ad60c.png)](https://imgse.com/i/p9Mx7iF)

这时会自动进入一个文件管理器窗口，并且在Ubuntu桌面左边应用栏会显示一个DVD光盘图标，这个DVD光盘就是VMware tools的光盘文件包。这里面最重要的就是那个压缩文件，待会用到这个文件进行VMware tools的安装。

将这个压缩包复制到桌面

[![p9QDO1I.png](https://img-blog.csdnimg.cn/img_convert/44b5444d943af405f210697c3c5dcd07.png)](https://imgse.com/i/p9QDO1I)

[![p9Qy0Xt.png](https://img-blog.csdnimg.cn/img_convert/ba52cb493a048af212046991d8e19281.png)](https://imgse.com/i/p9Qy0Xt)

对桌面上这个压缩包进行解压缩：

用快捷键ctrl alt t 打开终端，输入命令对该压缩包解压，命令是

```
tar zxf  VMwareTools-10.3.22-15902021.tar.gz
```

[![p9QcKqx.png](https://img-blog.csdnimg.cn/img_convert/dd17af6c5ab50db21720e1ed0427592b.png)](https://imgse.com/i/p9QcKqx)

回车，会在桌面显示一个名为vmware-tools-distrib的解压后的文件夹👇

[![p9QcGJe.png](https://img-blog.csdnimg.cn/img_convert/de0ee44e02f56cbd490f0fbbcc43a9cb.png)](https://imgse.com/i/p9QcGJe)``

打开该文件夹，可以看到一个叫vmware-install.pl的文件夹，这个文件夹就是安装文件

[![p9QcgQs.png](https://img-blog.csdnimg.cn/img_convert/80c1cd65e7f9ae5d44b469f31d10f634.png)](https://imgse.com/i/p9QcgQs)

在终端中运行该文件，如下

[![p9QgAmt.png](https://img-blog.csdnimg.cn/img_convert/6b931784c592b4a0e29dca03d4354031.png)](https://imgse.com/i/p9QgAmt)

发现终端提示请使用超级管理员模式运行↓

[![p9QgVTf.png](https://img-blog.csdnimg.cn/img_convert/4f8baabe98ce6174df87175ced065214.png)](https://imgse.com/i/p9QgVTf)

进入超级管理员模式 ↓：输入命令sudo su，回车后输入用户密码再回车进入超级管理员模式

[![p9QglXn.png](https://img-blog.csdnimg.cn/img_convert/ff1b830879929656f4dc405ec77b17e0.png)](https://imgse.com/i/p9QglXn)

再运行vmware-install.pl文件

[![p9Qgap4.png](https://img-blog.csdnimg.cn/img_convert/d7cd65f4a78f22ab59c4302f8a09f778.png)](https://imgse.com/i/p9Qgap4)

再第一次停顿处输入yes，回车之后其他的停顿处都按回车继续安装，直至终端窗口文字滚动完成，到如下这个截图样式就是表示安装完成

[![p9Qgr0x.png](https://img-blog.csdnimg.cn/img_convert/26f2bb1a19c3f380fd766ae5e8ca30ca.png)](https://imgse.com/i/p9Qgr0x)

<font size=5>**==注意！！！==**</font>
到这有可能还不能实现Windows主机与Ubuntu虚拟机之间的复制粘贴（我的就是这样😭），这个问题一直困扰着我好久，终于在不懈努力之下，再往上找到了一个解决办法，下面就分享给大家😉。

通过了解得知，之前安装VMware Tools之后仍然不能正常实现主机与Ubuntu之间的复制及文件拖拽的原因是，从Ubuntu14.04开始，“open-vm-tools”代替了官方的“VMware Tools”，所以要重新安装新的open-vm-tools。

<font size=5>**==新的解决方法：== 👇**</font>

  1. 首先将之前安装过的VMware Tools给卸载掉，找到VMware Tools的安装路径文件，在vmware-tools-distrib/bin文件夹下有一个名为vmware-uninstall-tools.pl的可执行文件，这个文件就是卸载VMware Tools的可执行文件，进入终端命令行执行以下命令：

```
sudo ./vmware-uninstall-tools.pl
```

[![p93OfCF.png](https://img-blog.csdnimg.cn/img_convert/73e81e23abb464d91356d03eff7095ce.png)](https://imgse.com/i/p93OfCF)

卸载完成后，安装open-vm-tools。依次执行以下命令👇

```go
sudo su //进入到管理员模式
apt-get install open-vm-tools open-vm-tools-desktop //安装命令
vmware-user //开启服务
```