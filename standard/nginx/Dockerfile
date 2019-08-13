FROM nginx:alpine

# 删除默认站点配置
RUN rm -f /etc/nginx/conf.d/default.conf \
    && apk add tzdata curl \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && apk del \
        tzdata\
        curl \