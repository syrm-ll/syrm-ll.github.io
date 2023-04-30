# 一. Linux 基础

## 登录退出关机重启休眠

```shell
# shutdown -h {T} 关机
shutdown -h now 
# shutdown -r {T} 重启
shutdown -r now 
# reboot 重启
reboot
# 同步， 内存数据写入硬盘
sync

# 退出登录
logout

# 退出shell
exit
```

## 运行级别

```shell
# 0 关机，1 单用户，2 多用户 无网络服务，3 多用户有网络服务，4 保留，5 图形界面，6 重启
init 0-6

# 查看当前运行级别
systemctl get-default
# 设置运行级别
systemctl set-default graphical.target | multi-user.target
```

## 用户和用户组

### 用户管理

```shell
# 查询用户信息
id {用户名}

# 切换用户
su {用户名}


# 新建用户，自动生成家目录，/home/{用户名}
useradd {用户名}
# 新建用户，指定家目录
useradd -d {URL} {用户名}
# 修改/重置密码，用户名为空默认给当前用户修改密码。 sudo passwd root 可以给root重置密码
passwd [用户名]


# 删除用户，但是保留用户目录
userdel {用户名}
# 删除用户，同时删除用户目录
userdel -r {用户名}


# 查询登录信息
whoami
who am i
```



### 用户组管理

```shell
# 添加组 详细见 -h 帮助
groupadd
# 删除组
groupdel
```

### 权限相关

```shell
# 修改文件所有者
chown {用户名} {URL}

# 修改文件权限
# rwx=421
chmod u=rwx,g=rw,o=x {文件名}
chmod 777 {文件名}
```



### 用户相关的配置文件

```shell
# /etc/passwd
用户名 : 口令 : 用户标识UID : 组标识 GID : 注释 : 主目录 : 登录shell

# /etc/shadow
登录名 : 口令密文 : 最后一次修改时间 : 最小时间间隔 : 最大时间间隔 : 警告时间 : 不活动时间 : 失效时间 : 标志

# /etc/group
组名 : 口令 : 组标识 : 组内用户列表
```



## 文件和目录

### 文件目录管理

cd pwd mkdir touch echo cat cp mv rm ls 略

```shell
# ln 连接

# 搜索查找
find 
# grep 管道搜索
ps -ef | grep XXX
```

### 压缩和解压

```shell
# gzip, gzip 压缩 解压缩会删除原文件
# 所有 *.gz 文件
gzip {文件名} # 压缩
gunzip {gz压缩文件名} # 解压

# zip
# 所有 *.zip 文件
zip {zip压缩文件名} {目标文件路径}
unzip [目标路径:默认当前路径] {zip压缩文件名}

# tar
# 所有 *.tar.gz 文件
# -c tar 打包，-z 打包时候压缩, -v 显示信息, -f 指定生成文件名
# -x 解压
tar -czvf XXX.tar.gz {原文件} 			# 压缩
tar -xvf {XXX.tar.gz} [ -C 目标路径 ]	# 解压
tar -zxvf {XXX.tar.gz} [-C URL ]		# 解压
```





## 时间

```shell
# 显示系统时间
date
# 格式化时间
date "+%Y-%m-%d %H:%M:%S"

# 显示日历
calps
```



## 定时任务 

```shell
# cron 循环任务
# 编辑任务
crontab -e
*/2 * * * * ls -Alh > /tmp/to.txt

# atd 单次任务

```

## 磁盘分区

```shell
# 查看分区信息
lsblk
lsblk -f

# 分区
fdisk

# 格式化
# mkfs -t {FS type} {分区URL}
mkfs -t ext4 /dev/sdb1

# 挂载
# mount {设备URL} {目标URL（挂载点）}
mount /dev/sdb1 /mnt/hdddisk
# 卸载
umount {设备URL} | {挂载点URL}

# 永久挂载
修改 /etc/fstab 文件

# 查看磁盘占用量
df -h [URL]
du -h --max-depth=1 URL

# 文件筛选
ll | grep "^d" && ll | grep "^-"

```

## 网络管理

### 网络配置

```shell
# centos 
# /etc/sysconfig/network-scripts/网卡名称


# 主机名 和 host
/etc/hostname 	# 文件
/etc/hosts 		# 文件


# windows DNS 缓存查看
# ipconfig /? 帮助
ipconfig /displaydns
ipconfig /flushdns
```

### 网络监控

```shell
netstat

# 按顺序输出
netstat -an
# -p 额外显示进程名
netstat -anp 

# 网络联通测试
ping 
```







## 任务管理

```shell
# 查看进程信息
# -a 全部，-u 用户， -x 后台，-e 全部， -f 完整
ps -aux
ps -ef

# 终止进程
# kill [选项] PID 
# -9 强制
kill -9 1
# 结束指定进程和它全部子进程
killall PID

# pstree 查看进程树
```

## 服务管理

```shell
# centos 7.0 之后不推荐
# /etc/init.d 下的可执行文件
# setup 查看所有可管理系统服务。（仅限centos）
service 服务名 <start | stop | restart | reload | status >

# 优先使用
# /usr/lib/systemd/system
systemctl < start | stop | restart | status > 服务名
# 查看开机自动列表
systemctl list-unit-files 
# 设置，查询开机自启状态
systemctl enable 服务名
systemctl disable 服务名
systemctl is-enabled 服务名


# 查看服务在不同的运行级别下的启动状态 (仅限Centos)
chkconfig 
```

> 防火墙服务名：firewald
> `systemctl enable firewald`
>
> ```shell
> # 打开端口
> firewall-cmd --permanent --add-port=端口号/协议
> # 关闭端口
> firewall-cmd --permanent --remove-port=端口号/协议
> 
> # 重新载入
> firewall-cmd --reload
> 
> # 查询
> firewall-cmd --query-port=端口/协议
> ```



# 二. 发行版

## Centos
## Ubuntu

## 查看系统版本信息

```shell
# 命令
cat /proc/version
# 显示如下
Linux version 4.10.0-28-generic (buildd@lgw01-12)         # linux内核版本号
gcc version 5.4.0                                         # gcc编译器版本号
Ubuntu 5.4.0-6ubuntu1                                     # ubuntu版本号
```



```shell
uname -a
# 显示linux的内核版本和系统是多少位的：X86_64代表系统是64位的。
```



```shell
# 输入命令
lsb_release -a

# 显示如下
Distributor ID: Ubuntu                    # 类别是ubuntu
Description:  Ubuntu 16.04.3 LTS          # 16年3月发布的稳定版本，LTS是Long Term Support：长时间支持版本，支持周期长达三至五年
Release:    16.04                         # 发行日期或者是发行版本号
Codename:   xenial                        # ubuntu的代号名称
```



## 软件源



### 清华软件源

https://mirror.tuna.tsinghua.edu.cn/help/ubuntu/

18.04 LTS 

```shell
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```



## 安装GCC

```shell
sudo apt update
# 默认的Ubuntu存储库包含一个名为build-essential的元包，它包含GCC编译器以及编译软件所需的许多库和其他实用程序。
sudo apt install build-essential
```



验证安装：`gcc -v`



## GCC 官网：

https://gcc.gnu.org/onlinedocs/



## 初始化

### 更改系统语言

```shell
# 查看系统语言
echo  $LANG

# 查看当前语言包
locale -a

# 如果没有(en_US)?.uft8 需要安装
$ sudo apt install language-pack-zh-hans

# 再次查看语言包
locale -a
----------------------------------------------------------------------------------------------------
# 更改单个用户 

# 编辑，添加 LANG="zh_CN.utf8"
vim ~/.bashrc

# 重新加载文件
source ~/.bashrc
----------------------------------------------------------------------------------------------------
# 更改全局

sudo vim /etc/default/locale

LANG="zh_CN.UTF-8"
LANGUAGE="zh_CN:zh"
LC_ALL="zh_CN.UTF-8"


```



## deepin / UOS



# 三. 历史归档

## 查看系统信息

## 查看内核版本

```shell
$ cat /proc/version
Linux version 3.10.0-1062.9.1.el7.x86_64 (mockbuild@kbuilder.bsys.centos.org)
(gcc version 4.8.5 20150623 (Red Hat 4.8.5-39) (GCC) ) #1 SMP Fri Dec 6 15:49:49 UTC 2019

# 或

$ uname -a
Linux centos_pc 3.10.0-1062.9.1.el7.x86_64 #1 SMP Fri Dec 6 15:49:49 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
```



## 查看系统发行版本

```shell
# 使用第三方工具，如果没有需要安装
$ lsb_release -a
LSB Version:    :core-4.1-amd64:core-4.1-noarch:cxx-4.1-amd64:cxx-4.1-noarch:desktop-4.1-amd64:desktop-4.1-noarch:languages-4.1-amd64:languages-4.1-noarch:printing-4.1-amd64:printing-4.1-noarch
Distributor ID: CentOS
Description:    CentOS Linux release 7.7.1908 (Core)
Release:        7.7.1908
Codename:       Core


# 对于Redhat系的Linux
$ cat /etc/redhat-release
CentOS Linux release 7.7.1908 (Core)
```



## 修改系统设置

### 软件源

### centos：yum

```shell
# 针对 centos 8
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-8.repo
yum makecache
```

参考： https://developer.aliyun.com/mirror/centos?spm=a2c6h.13651102.0.0.3e221b11CGdINm



### centos：dnf

略

### Ubuntu：apt

```shell
# ubuntu 18.04(bionic) 配置如下
# /etc/apt/sources.list
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```

参考：https://developer.aliyun.com/mirror/ubuntu?spm=a2c6h.13651102.0.0.3e221b11CGdINm

### 语言和时区

### 设置系统语言为中文

配置文件位置 `/etc/locale.conf`

**centos 8：**

```shell
yum install glibc-common
yum install -y langpacks-zh_CN
vim /etc/locale.conf   # 修改这个文件
	LANG=zh_CN.utf8
source /etc/locale.conf 
```



**验证：**

```shell
echo $LANG
```

## 环境变量

配置文件位置： `/etc/profile`