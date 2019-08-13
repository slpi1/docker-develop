# php镜像制作说明

php的镜像提供两个版本，具体差异查看Dockerfile:
- [php:7.2-fpm](./Dockerfile)
- [php:7.2-fpm-alpine](./Dockerfile.alpine)

官方提供的基础镜像包含以下扩展：
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

本次构建会新增下列扩展：

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