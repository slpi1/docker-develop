# laravel应用中如何运行计划任务及队列

## 问题分析
假设应用已经存在的容器有：
- nginx: web服务容器，处理http请求
- php-fpm: php-fpm容器，处理由nginx转发的动态请求
- mysql: mysql容器，提供数据服务
- redis: redis容器，提供redis缓存服务

如果要实现计划任务及队列的运行，有两个思路：
- 借助已有的php-fpm容器中运行
- 重新构建一个容器来运行

如果借助已有的php-fpm容器来运行，理论上是可行的，但是如果应用需要被部署多套，那么多个php-fpm的容器中都会有crond的进程，可能会导致任务重复运行，所以我们应该通过第二个思路来进行。

## 实施过程
我们需要再构建一个queue的容器，来运行计划任务及队列，容器中需要包含php的运行环境，所以该容器的镜像，可以基于 `harbor.slpi1.com/information/php:7.2-fpm-alpine`。容器执行计划脚本，需要更新crontab的计划；容器需要运行队列，一般通过supervisor来管理队列运行进程。需要注意的是，通过上述分析，我们知道该容器中实际上需要运行两个进程，一个是crond，一个是supervisord，解决这个问题也有两个办法：
- 指定 `ENTRYPOINT` 文件，在其中启动两个进程
- 通过supervisord来启动crond

我们选择通过supercisord来启动crond，据此，我们编写出的Dockerfile如下：

```
FROM harbor.slpi1.com/information/php:7.2-fpm-alpine

RUN \
    # 这里加入计划任务
    echo "* * * * * php /app/artisan schedule:run >> /dev/null 2>&1" >> /etc/crontabs/root \

    # 安装supervisor
    && apk add supervisor \

    # 清理安装后文件
    && rm /var/cache/apk/* \

    # 修改supervisor的运行模式。
    # supervisord是Docker容器中的1号进程，也需要始终保持运行状态。
    # nodaemon设为true时，表示supervisor保持前台运行而非在后台运行。
    # 若supervisor在后台运行，则Docker容器也会在执行supervisord命令后立即Exited.
    && sed -i 's/^\(\[supervisord\]\)$/\1\nnodaemon=true/' /etc/supervisord.conf

COPY supervisor-worker.ini /etc/supervisor.d/

CMD ["supervisord", "-c", "/etc/supervisord.conf"]
```

`supervisor-work.ini` 内容如下:

```
[program:cornd]
command=/usr/sbin/crond -f

[program:queue]
process_name=%(program_name)s_%(process_num)02d
command=php /app/artisan queue:work --sleep=3 --tries=3
autostart=true
autorestart=true
user=root
numprocs=8
redirect_stderr=true
stdout_logfile=/app/storage/logs/worker.log
```