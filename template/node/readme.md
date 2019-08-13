# node应用构建说明

构建前准备好下列文件备用：
- domain.conf: 站点nginx配置文件。
- Dockerfile-nginx: nginx容器构建文件。
- docker-compose.yml: 容器编排文件。

**构建过程会根据项目具体情况而有所差异。**

# domain.conf

# Dockerfile-nginx
通过多阶段构建的方法，构建前端web容器。

# docker-compose.yml