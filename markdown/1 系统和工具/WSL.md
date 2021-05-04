# 初始化



# 配置 java 开发环境



## Docker

### IDEA 使用 wsl

IDEA 全局设置 > 工具 》 终端 》 应用程序设置 》 shell路径：

```shell
"cmd.exe" /k "wsl.exe"
```

### IDEA 使用 docker

> docker 安装后开放远程端口， IDEA 直接配置即可。

编辑 `/etc/default/docker` 添加

```shell
# 开启远程访问 -H tcp://0.0.0.0:2375
# 开启本地套接字访问 -H unix:///var/run/docker.sock
DOCKER_OPTS="-H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock"
```



IDEA 设置：

IDEA 全局设置 》构建 执行 部署 》 docker 》添加

TCP 套接字:

```shell
tcp://localhost:2337
```



