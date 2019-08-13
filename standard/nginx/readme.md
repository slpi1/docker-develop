# nginx镜像制作说明

原始镜像为nginx:alpine，仅仅对原始镜像的默认站点配置文件进行删除。在基于该镜像创建容器之前，需要手动拷贝站点配置文件到容器站点配置目录 `/etc/nginx/conf.d/`：

```
// Dockerfile

COPY web.conf /etc/nginx/conf.d;
```