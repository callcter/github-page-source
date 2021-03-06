---
title: linux常用命令
tags:
  - Linux
url: 389.html
id: 389
categories:
  - 服务器
date: 2017-04-01 18:50:02
---

不做长篇大论，仅罗列一下自己常用到的指令，以作备忘

### ls

    ls  -- 罗列当前路径下的可见文件、文件夹
    ls -al  -- 这是最常用到的指令，显示全部文件（包括隐藏），并显示详细信息（包括权限、所有者、所有组、修改时间等）
    

### cd

    cd /dir/dir -- 进入某个目录
    cd ..  -- 回到上一层
    cd  -- 回到用户目录
    

### pwd

    pwd  -- 显示当前所在路径
    

### chmod

变更文件权限，分为 r（读取，4分），w（写入，2分），x（执行，1分），权限分为三组，所有者、群组、其他用户，以 rw-r--r--，意思是所有者有读写权限，群组内的用户和其他用户只有读取的权限

    chmod 777 file -- 当前文件给所有者、群组、其他用户设置拥有全部权限
    chmod 777 file -R -- 递归处理，当前文件及其子目录下全部文件给所有者、群组、其他用户设置拥有全部权限
    

### chown

更改文件的所有者

    chown user file -R  -- 递归处理，当前文件及其子目录下全部文件所有者修改为 user 
    

### cp

复制

### mv

移动

### mkdir

创建文件目录

### rm

移除文件

### ssh

远程连接

    ssh -p 21 root@110.110.110.110  -- 以 root 账号连接地址为 110.110.110.110:21 的服务器

### sed grep

sed -i "" "s/com.agan_app/com.agan_app.prod/g" `grep com.agan_app -rl ./prod`

### 参考文章

- [linux 批量查找与替换](https://www.cnblogs.com/kongzhongqijing/articles/8459069.html)
- [mac环境使用sed修改文件出错的解决方法](https://blog.csdn.net/fdipzone/article/details/51253955)
- [Mac 运行sh文件，也就是传说中的shell脚本](https://blog.csdn.net/yusufolu9/article/details/53706269)