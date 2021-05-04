# 安装

## 方法一： 使用官方脚本

```shell
$ wget -qO- https://get.docker.com/ | sh
# 这里会从官网下载一个脚本，然后运行安装。
```



## 方法二：参考如下教程

### centos

https://www.cnblogs.com/qgc1995/archive/2018/08/29/9553572.html

**或者使用：**

```shell
# 1，更新yum
sudo yum update

# 2，安装需要的软件包
sudo yum install -y yum-utils device-mapper-persistent-data lvm2

# 3，设置yum源为阿里云
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

# 4，安装docker
sudo yum install docker-ce

# 5，验证安装
docker -v
```



### Ubuntu / deepin / UOS

参考： https://www.jianshu.com/p/8200a3a50806

## 验证安装

```shell
$ docker version

Client: Docker Engine - Community
 Version:           19.03.8
 API version:       1.40
 Go version:        go1.12.17
 Git commit:        afacb8b
 Built:             Wed Mar 11 01:27:04 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.8
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.17
  Git commit:       afacb8b
  Built:            Wed Mar 11 01:25:42 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.2.13
  GitCommit:        7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```



# 配置镜像源

登录阿里云账户， 在 `【容器镜像服务】` - `【镜像加速器】` 中找到自己的地址， 按照说明配置即可。

> ## 1. 安装／升级Docker客户端
>
> 推荐安装1.10.0以上版本的Docker客户端，参考文档 [docker-ce](https://yq.aliyun.com/articles/110806)
>
> ## 2. 配置镜像加速器
>
> 针对Docker客户端版本大于 1.10.0 的用户
>
> 您可以通过修改daemon配置文件/etc/docker/daemon.json来使用加速器
>
> ```shell
> sudo mkdir -p /etc/docker
> sudo tee /etc/docker/daemon.json <<-'EOF'
> {
>   "registry-mirrors": ["https://jh8ymifg.mirror.aliyuncs.com"]
> }
> EOF
> sudo systemctl daemon-reload
> sudo systemctl restart docker
> ```
>



# 使用

安装之后的docker 使用 `docker` 的服务名， 可以使用 `systemctl` 管理：

```shell
# 启动
systemctl start docker
# 重启
systemctl start docker
# 查看状态
systemctl status docker
# 停止
systemctl stop docker

# 设置开机自启
systemctl enable docker
```



## 镜像相关

```shell
# 概要
docker info
# 帮助文档
docker -help

# ---------------------------------------------------
# 搜索镜像 -s 筛选
docker search [-s nunber] <image-name>
docker search --filter=stars=number <image-name>

# pull / 拉取 / 下载
docker pull [imagesName]

# 查询本地镜像列表， q 只查看ID， a 查询所有（含中间层）
docker images -qa

# 删除镜像
docker rmi [image-name] [name] [name]
docker rmi -f $(docker images -qa)
# 删除所有
docker rmi `docker images -q`
```







## 容器相关

```shell
# 查看正在运行的容器
	-a 全部
	-q 只显示ID
	-n 最近N个容器
	-l 最近一个容器
docker ps [-aqnl]


# 创建并运行容器
# options:
	--name: 为容器指定名称
	-d: 后台运行并返回容器ID
	-i: 交互式运行。
	-t: 为容器重新分配一个伪终端。
	-P: 随机端口映射。
	-p: 指定端口映射
	
	-v: 映射虚拟卷
docker run [options] images-name [command] [arg...]

# 启动，重启，停止，强制关闭
docker start 容器名
docker restart 容器名
docker stop 容器名
docker kill 容器名

# 删除容器
docker rm 容器名/ID
# 全部删除
docker rm $(docker ps -aq)
docker ps -aq | xargs docker rm

# 重新进入容器。
docker exec -it 容器ID bashshell
docker attach 容器ID
```



## 容器管理

```shell
# 打印指定容器的日志
docker logs 容器ID 

# 从容器复制文件到主机
docker cp 容器ID:URL URL

# 添加虚拟卷
docker run -it -v <宿主机绝对路径>:<容器内绝对路径> <image-name>

# 添加容器卷
docker run -it --volumeslfrom <name/id> <images>


```



## dockerfile

dockerfile文件

docker buider

docker run



### dockerFile 保留字指令

```yml
# 父级镜像名 scratch bash 镜像 为超级父类镜像
from scratch
# 镜像作者的姓名和邮箱
maintainer
# 容器构建时候需要运行的命令
run
# 当前容器对外暴露的端口号
expose
# 工作文件夹， 指定登陆后进入系统的默认目录。
workdir

# 添加自定义环境变量，可以在其他RUN命令中使用#name 引用。
env

# 把宿主机文件复制进镜像，处理过程中会处理链接并解压缩。
add
# 类似add，复制文件到镜像中。 
copy


# 容器数据卷， 用于数据持久化。 和run -v 类似。
volume
# 指定容器启动时候运行的CMD命令， 可以有多个CMD，但是只有最后一个生效。
cmd
# 类似cmd，指定容器启动时候运行的命令。
entrypoint
# 当前镜像被其他镜像继承，执行构造时候触发onbuild运行。
onbuild

```

### 自定义centos

```dockerfile
from centos
maintainer 作者<youxiang@email.com>
env workdir /root/work
workdir $workdir

run yum -y install vim
run yum -y install net-tools
# iputils-ping

expose 80

cmd /bin/bash
```



### 构建

```dockerfile
docker build -f <dockerfileUrl> -t <imageName> .
```

> 注意： 最后有一个点， 原因不明， 大意指上下文。





## k8s+gitlab+jenkins



# 安装mysql

## 拉取镜像

```shell
docker pull mysql
```

## 启动容器

```shell
# -e 参数，MYSQL_ROOT_PASSWORD=123456
docker run -itd --name mysql-test -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql

-v /data/mysql/my.cnf:/etc/mysql/my.conf -v /data/mysql/data:/var/lib/mysql
```

## 其他信息

```yaml
默认端口: 3306
基础系统: Debian
数据目录: /var/lib/mysql/
# -h 主机名，-P 端口， -u 用户 -p 密码 
连接: mysql -h localhost -P 3306 -u root -p
```

# 安装sqlserver

## 镜像名：

```shell
docker search mssql
docker pull mcr.microsoft.com/mssql/server:2017-latest
```

## 启动命令：

```shell
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=sa1122.?' -p 1433:1433 -v /opt/data/sqlserver/:/var/opt/mssql/data/ --name sqlserver -d mcr.microsoft.com/mssql/server:2017-latest

# 注意
# 密码复杂度要求：
# 		最低8个字符
#		大写字母，小写字母，十进制数字，字符。 包含其中三组。
# 不满足密码要求，启动失败。
```

## 其他信息：

```yml
默认端口: 1433
连接地址: localhost 和 127 不能用，原因不清楚。  ::1 可以
数据目录: /var/opt/mssql/data/
基础系统: ubuntu
客户端工具: /opt/mssql-tools/bin/sqlcmd
```

