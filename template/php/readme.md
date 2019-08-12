# php应用构建说明

构建前准备好下列文件备用：
- domain.conf: 站点nginx配置文件。
- Dockerfile-nginx: nginx容器构建文件。
- Dockerfile-php: php容器构建文件。
- docker-compose.yml: 容器编排文件。

# domain.conf

站点配置文件中需要注意fastcgi_pass配置的值与docker-compose.yml中的服务名保持一致:

```
# domain.conf
fastcgi_pass   laravel:9000;

#docker-compose.yml
services:

  laravel:
    build: 
      context: .
      dockerfile: Dockerfile-php
    networks:
```

# Dockerfile-nginx
nginx容器构建文件。主要是将domain.conf配置文件添加到nginx配置目录，然后将应用入口文件拷贝到站点根目录。

# Dockerfile-php
php容器构建文件。应用依赖安装，应用代码拷贝。

# docker-compose.yml
容器编排文件.