# redis镜像制作说明

[Dockerfile](./Dockerfile)

redis的数据存储目录默认在容器的 `/data` 目录。通过以下方法开启持久化：
```
$ docker run --name some-redis -d -p 6379:6379 redis redis-server --appendonly yes
```

配合容器挂载宿主机的目录，可以将数据持久化到宿主机，避免容器被删除导致的数据丢失。