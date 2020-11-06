# php7-env-bin php7+apache 二进制bin文件直接运行包 绿色版Linux php运行环境
## 简介
php7-env 是Linux CentOS7 下apache+php一键部署环境包(php-7.2.99 apache-2.4.10)，绿色版，直接二进制解压即可运行。与原服务环境无任何冲突,不写系统环境变量，不影响系统已安装的web服务器(nginx、apache等)，内置apache如启动只默认占用1038端口，内置redis、swoole、rdkafka、zookeeper的php扩展，也可以使用phpize安装更多扩展,默认解压到/opt/目录下。该环境主要用于快速部署应用。
## 安装说明
下载release包
wget https://github.com/io3x/php7-env-bin/releases/download/a5728a7/io3x-env-php7299-apache2410-bin-v1.0.zip
系统环境支持 CentOS7+
上传bin包文件到系统/opt目录
![](https://php-images.oss-cn-qingdao.aliyuncs.com/uploadfile/md/202010/1017/95E6A2A60732A0220D031F7E9AD84AA2.png)
执行
> unzip io3x-env-php7299-apache2410-bin-v1.0.zip

![](https://php-images.oss-cn-qingdao.aliyuncs.com/uploadfile/md/202010/1017/48623CEC4046725074EA1040356263D4.png)

### 安装系统环境依赖
    yum -y install zip unzip libyaml-devel libxml2 libxml2-devel openssl openssl-devel curl curl-devel libjpeg libjpeg-devel libpng libpng-devel freetype-devel libxslt libxslt-devel bzip2


### 指定 zookeeper、rakafka 扩展包依赖库
执行
> echo '/opt/io3x-env/lib/lib'>/etc/ld.so.conf.d/io3x-env.conf

执行
> ldconfig

立即刷新环境变量

### PHP信息及默认扩展

    [root@localhost bin]# /opt/io3x-env/php7/bin/php -v
    PHP 7.2.34 (cli) (built: Oct 14 2020 01:15:34) ( NTS )
    Copyright (c) 1997-2018 The PHP Group
    Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
        with Zend OPcache v7.2.34, Copyright (c) 1999-2018, by Zend Technologies
    [root@localhost bin]# /opt/io3x-env/php7/bin/php -m
    [PHP Modules]
    bcmath
    Core
    ctype
    curl
    date
    dom
    fileinfo
    filter
    gd
    gettext
    hash
    iconv
    json
    libxml
    mbstring
    mongodb
    mysqli
    mysqlnd
    openssl
    pcntl
    pcre
    PDO
    pdo_mysql
    pdo_sqlite
    Phar
    posix
    rdkafka
    redis
    Reflection
    session
    shmop
    SimpleXML
    soap
    sockets
    SPL
    sqlite3
    standard
    swoole
    sysvsem
    tokenizer
    xml
    xmlreader
    xmlrpc
    xmlwriter
    xsl
    yaml
    Zend OPcache
    zip
    zlib
    zookeeper
    
    [Zend Modules]
    Zend OPcache


### 安装更多扩展
在解压后的扩展目录执行
> /opt/io3x-env/php7/bin/phpize
./configure --with-php-config=/opt/io3x-env/php7/bin/php-config
make && make install

打开
/opt/io3x-env/php7/etc/php.ini
添加
extension=xxx.so
即可

## 使用apache httpd服务器
可以配置多个端口

### 启动httpd web服务器
转到目录,执行
> cd /opt/io3x-env/httpd && ./apachectl start

即可

访问
http://192.168.0.102:1038/
如不能打开，看是否关闭防火墙
> service firewalld stop

![](https://php-images.oss-cn-qingdao.aliyuncs.com/uploadfile/md/202010/1017/579A5A9F5D7B1C3D550D90BC120B1F1A.png)

### 编辑站点
转到
/opt/io3x-env/httpd/conf/vhosts
目录下，复制一个 default.conf 做对应修改即可

## 作为服务器主要环境部署
如果作为服务器主要环境部署,需要修改用户组和执行用户 默认是root（仅仅为了安全起见）

把php等可执行程序加入环境变量


    cat >> /etc/profile << "EOF"
        export PATH=$PATH:/opt/io3x-env/php7/bin
        EOF

刷新环境变量使之生效
> source /etc/profile

主要如果是crontab任务计划中执行php,需要用到绝对路径
/opt/io3x-env/php7/bin/php index.php
不能直接使用php index.php

## 配和nginx使用
直接设置nginx反向代理到apache的端口即可，当然也可以直接通过apache配置好域名和端口,只是得占用80端口了





