# 配置文件

events

work_connections 1024;   每个工作进程的最大连接数。

use   epoll  ;  内核模型， select | poll | epoll ; 推荐 epoll



```shell
# 日志信息格式
$remote_addr  // 客户端地址
$remote_user // nginx 认证用户名
$time_local  //本机时间
$request //http请求行
$status  // http 状态码
$body_bytes_sent  // 响应正文大小
$http_referer   // 上一级页面， 表示用户从何处来。 防盗链。
$http_user_agent  // HTTP 头信息， 客户端访问设备。
$http_x_forwaeded_for // http 请求携带的信息。



# 默认的server
    server {
        # 单连接请求上限次数。
        keepalive_requests 120;
        # 监听端口和地址。
        listen       80;
        server_name  localhost;

        #charset koi8-r;
        location /data {
		alias   /home/uos/Documents/data;
                autoindex on;
                autoindex_exact_size off;
                autoindex_localtime on;
        }
       

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }


```

## 作为下载站点， 列出目录

```c
可以配置在http  server  location

autoindex off | on; // 列出目录
autoindex_exact_size on| off; // 显示字节 | KB，MB，GB
autoindex_local_time off | on; // 显示文件时间为GMT | 文件服务器时间。
charset utf-8,gbk; // 字符集。
```

## 访问限制

请求限制，定义在http内， 在location内引用。



```shell
# 连接限制
limit_conn_module 
# 请求限制
limit_req_modeule

```



## 跨域访问

配置nginx使用反向代理解决跨域问题， 可能会遇到前端静态资源文件无法访问。

在location中加入如下配置即可。

```shell
add_header Access-Control-Allow-Origin *;  
add_header Access-Control-Allow-Credentials true;
add_header Access-Control-Allow-Methods GET,POST,OPTIONS;
```

## SSI 服务器端包含

SSI配置在虚拟主机`server` 中。

```shell
# ssi 开关
ssi on;
# 错误日志开关
ssi_silent_errors on;
# 默认为text/html 如果需要支持shtml脚本， 需要改成text/shtml
ssi_types text/html;
```

**使用**

在html页面中，使用如下指令即可引入被包含文件：

```html
<!--#include virtual="<URL>"-->
```





## 负载均衡

在 `http` 块中增加名称为myservername的upstream。

server location proxy_pass  指向这个upstream。

```c++
upstream myservername {
    [ip_hash;]
    server    host:port  [weight=1];
    server    host:port  [weight=3];
    [fail]
}

server {
    listen    90;
    server_name    name;
    
    location / {
        root   html;
        index   index.html;
        proxy_pass   http://myservername
    }
}
```

# 配置高可用

使用 `keepalived`  添加虚拟IP



