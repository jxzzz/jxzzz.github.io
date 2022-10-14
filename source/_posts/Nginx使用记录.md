---
title: Nginx使用记录
date: 2022-10-13 21:05:48
tags:
---

### 基础命令

```bash
nginx -s stop
nginx -s quit
nginx -s reload  // 重新生成一个log文件
```

### nginx 配置记录

#### 配置一个反向代理

```bash
server {
    listen 80;
    server_name name;

    location / {
        proxy_pass http://127.0.0.1:8800; # 所有请求的服务都和转发到该地址
        rewrite "^/api/(.*)$" /$1 break;  # break 至今发起请求  last 重新配配发起请求
    }
}

# "^/api/(.*)$"  从/api/开始匹配捕获后面的参数
```

### 负载均衡 基本配置

```bash
http {
     upstream tomcat {
        server 192.168.25.148:8081;
        server 192.168.25.148:8082 weight=2;  # 权重
    }

    server {
        listen 80;
        server_name "";

        location / {
            proxy_pass http://tomcat;   # edit ths

        }
    }
}

```
