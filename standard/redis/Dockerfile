FROM redis:4.0-alpine

RUN apk add tzdata curl \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && apk del \
        tzdata\
        curl