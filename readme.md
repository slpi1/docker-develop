# 信息化中心 - docker基础镜像

## 概述
本项目记录信息化中容器化实践过程中的问题。

## 镜像
出于统一应用的运行时环境、便于维护管理环境的目的，在实践过程中，产生的每个镜像，尽可能的源于同一个稳定的开源的镜像。在定义好所需要的基础镜像后，维护一套在部门层面统一通用镜像。

|基础镜像|标准镜像|Dockerfile|定制说明|镜像地址|
|---|---|---|---|---|
|nginx:alpine|nginx|待完成|无||
|php:7.2-fpm-alpine |php| [查看](standard/php/Dockerfile.alpine)|添加php常用扩展||
|composer:latest|composer|待完成|无||
|redis:4.0-alpine|redis|待完成|无||
|mysql:5.7|mysql|待完成|无||
|node:9-alpine|node|待完成|无||

标准镜像是经过部门标准化处理后的镜像，目的是为了去掉基础镜像所携带的版本号，简化镜像名称，同时也表名所运用的技术统一的立场。从基础镜像到标准镜像，会维护一套对应的Dockerfile，是根据具体需求所进行的定制化，在镜像标准化之后一般不对这部分的Dockerfile做更新。

## 应用
应用指日常编码的结果。应用的运行，建立在标准镜像之上。构建应用的运行时环境，需要考虑到实际应用的业务场景及需求，然后通过应用的Dockerfile文件定义所需要的容器。

应用层的Dockerfile并不是一层不变的，同时又是同类型相识的。基于这一事实，我们可以按应用类型维护一套Dockerfile模板，当需要开发新的应用时，在Dockerfile模板的基础上稍作修改，就可以定义各个容器,例如基于php的web应用，一般会需要`php/redis/mysql/nginx/node/composer`等容器。因此，Dockerfile应该包含以下过程：

- 定义redis运行容器
- 定义mysql运行容器
- 定义node运行容器，按照package.json文件安装js依赖，执行前端代码构建任务，生成前端静态资源文件。
- 定义composer运行容器，按照composer.json文件安装php依赖
- 定义php运行容器，拷贝应用代码到容器，拷贝php依赖到容器
- 定义nginx运行容器，配置应用站点，拷贝应用入口文件及前端静态资源文件到容器

### 应用的构建
docker可以根据应用的Dockerfile创建相应的容器，但还需要额外的工作来保证容器间的协作。通过Dockerfile创建容器并运行，得到应用能正常运行的环境，就是构建的过程。这一过程需要`.gitlab-ci.yml/docker-compose.yml`等文件来保证。