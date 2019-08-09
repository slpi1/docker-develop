# php镜像制作说明

## 基础镜像

```
FROM php:7.2-fpm-alpine
```

基础镜像包含以下扩展：
```
Core
ctype
curl
date
dom
fileinfo
filter
ftp
hash
iconv
json
libxml
mbstring
mysqlnd
openssl
pcre
PDO
pdo_sqlite
Phar
posix
readline
Reflection
session
SimpleXML
sodium
SPL
sqlite3
standard
tokenizer
xml
xmlreader
xmlwriter
zlib
```

## 扩展安装

```
bcmath
bz2
calendar
exif
gd
gettext
gmp
igbinary
imap
intl
ldap
mysqli
pdo_mysql
redis
soap
sockets
xmlrpc
xsl
zip
```