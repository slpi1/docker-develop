# nginx镜像制作说明

在基于该镜像创建容器之前，需要手动拷贝站点配置文件到容器站点配置目录 `/etc/nginx/conf.d/`，并覆盖默认配置：

```
// Dockerfile

COPY web.conf /etc/nginx/conf.d/default.conf;
```