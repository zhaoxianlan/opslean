#### yum报错（Linux更改python默认版本）

```
https://blog.csdn.net/zzq900503/article/details/79551715
 
whereis python --->python版本太多
which yum
 
1) vi /usr/bin/yum
#!/usr/bin/python --->#!/usr/bin/python2.6
 
File “/usr/sbin/yum-updatesd”, line 35, in
import dbus
 
ImportError: No module named dbus
 
则需要找到yum-updatest文件进行一样的修改，使用命令
 
which yum-updatest
vi /usr/bin/yum-updatest
 
ImportError: No module named urlgrabber.grabber
 
vi /usr/libexec/urlgrabber-ext-down
把头部的python改成和/usr/bin/yum中一样的
```

 

#### yum使用格式

yum [options] [command] [package ...]

###### option参数

-y：如果需要交互输入y的时候，自动输入

###### 清空操作

>clean [ packages | metadata | expire-cache | rpmdb | plugins | all ]
>
>作用：清空缓存的数据
>
>选项：
>
>   all：清空全部的数据【yum clean all】
>
>   packages：仅仅清空缓存的rpm包
>
>   metadata：仅仅清空缓存的元数据

###### 生成缓存

>makecache【yum makecache】

###### 查询操作

>1)repolist [all|enabled|disabled]   显示本机配置的yum仓库的信息
>
>   all：显示所有的yum源【yum repolist all】
>
>   enabled：显示可用的yum源（默认）
>
>   disabled：
>
>2)list [all|installed|available|updates]  显示软件的安装清空
>
>   all：显示全部的软件，包括已经安装和尚未安装
>
>   installed：仅仅显示已经安装rpm【 yum list installed】
>
>   available：仅仅显示尚未安装的rpm
>
>   updates：仅仅显示可以升级安装的rpm包
>
>   注意：
>
>       @表示软件已经安装
>    
>       anaconda：表示软件是在安装系统的过程中自动安装的
>
>3)info [name]: 显示一个rpm包的详细信息
>
>4)deplist [name]：显示指定软件的依赖包

centos6开始，在yum install a.rpm 的时候会首先在当前位置下找a.php，如果找不到才会去yum仓库中找

###### 安装升级和卸载

>    install ：安装软件【yum install 软件包名】
>
>    reinstall：覆盖安装（--replacepkgs）
>
>    update：升级软件，默认升级到最新版，如果要升级到指定版本，可以yum update soft-verion
>
>    check-update：检查可以进行升级软件包
>
>    downgrade：降级软件
>
>    remove：卸载软件【 yum remove 软件包+版本】
>
>    localinstall：

###### 和包组相关的

>   grouplist：显示系统中的包组的信息
>
>   groupinfo：查看一个包组中有哪些软件包
>
>   groupinstall：安装包组
>
>   groupremove：卸载包组

###### yum历史[回滚]

>yum history list 【rpm包名】
>
>显示某个事物的详细信息:
>
>yum history info ID
>
>yum history redo ID
>
>yum history undo ID

###### 只下载包

>yum install --downloadonly --downloaddir=/home/java   java
>
>rpm -Uvh --force /path/to/*.rpm