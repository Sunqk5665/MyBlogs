- ## 一、Linux基本命令
  ### 查看当前目录下内容：ls

  - 显示当前路径下的文件和文件夹
  - ls -l

    - 以列表的形式显示当前路径下的文件和文件夹

  - ls -lh

    - 以列表的形式显示当前路径下的文件和文件夹(文件大小带单位)

  - ls -a

    - 显示当前路径下的文件和文件夹包含隐藏文件（以.打头的文件为隐藏文件）

  ### 切换路径：cd

  - cd  文件夹路径
  - 跳转到家目录

    - cd
    - cd ~
    - cd /home/hq

  - cd ./

    - 在当前路径跳转，不改变操作路径(.表示当前)

  - cd ../

    - 跳转到上一级路径下面（..表示上一级）

  - cd -

    - 跳转到上一次操作路径，再执行就切换回原操作路径

  - 切换到根目录：cd /

  ### 新建文件

  - touch 文件名：创建普通文件
  - touch 同名文件：会更新时间戳

  ### 新建目录（文件夹）

  - mkdir 文件夹名：创建文件夹
  - mkdir aa/bb/cc -p

    - 以嵌套的方式创建多级目录

  ### 删除：rm

  - rm 文件名：删除普通文件
  - rm 文件夹名 -r：删除文件夹
  - rm * -r 删除当前目录下所有文件和目录
  - 删除前是否确认

    - -i  删除前逐一确认
    - -f 即使原档文件设为只读，也直接删除，无需逐一确认

  ### 复制：cp

  - cp 普通文件 路径

    - 拷贝普通文件到相应的路径下

  - cp 文件夹名 路径 -r

    - 拷贝文件夹到相应的路径下

  - 另存为

    - cp 文件名 路径/新文件名

  ### 移动：mv

  - mv 普通文件 路径

    - 移动普通文件到相应的路径下

  - mv 文件夹名 路径

    - 移动文件夹到相应的路径下

  - 重命名

    - mv 原文件名 新文件名

  ### 打印文件内容到终端：cat

  - cat 文件名.后缀

  ### 查看当前路径：pwd

  - 显示当前路径的绝对路径

    绝对路径：从根开始查找
    相对路径：从当前开始查找
