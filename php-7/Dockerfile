FROM registry.cn-hangzhou.aliyuncs.com/huati/centos7-gcc
MAINTAINER Leelmes "i@chengxuan.li"

# 安装依赖库
RUN yum install -y libxml2 libxml2-devel openssl-devel bzip2-devel libcurl-devel db4-devel gdbm-devel libwebp-devel libjpeg-turbo-devel libpng-devel libXpm-devel freetype-devel openldap-devel libmcrypt-devel mysql-devel sqlite-devel libtidy-devel libxslt-devel mysql-devel ImageMagick-devel libevent-devel libmemcached-devel libdb-devel tokyocabinet-devel mariadb-devel autoconf git svn make graphviz hiredis-devel jemalloc-devel && \
    yum clean all && \
    mkdir -p /data1/shell/

# 设置环境变量
ENV PATH /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# 安装PHP
RUN mkdir -p /data1/download && cd /data1/download  && \
    curl -o /data1/download/php-7.1.7.tar.gz http://kr1.php.net/distributions/php-7.1.7.tar.gz && \
    tar zxvf php-7.*.tar.gz && \
    cd  /data1/download/php-7.* && \
    './configure' '--prefix=/usr/local/' '--with-libdir=lib64' '--with-mysqli=shared,/usr/bin/mysql_config' '--with-sqlite3=shared' '--with-curl=shared,/usr/local/curl/' '--with-ldap=shared' '--with-xmlrpc=shared' '--with-zlib=shared' '--with-bz2=shared' '--with-iconv=shared' '--with-mhash=shared' '--with-mcrypt=shared' '--with-gd=shared' '--with-png-dir=/usr' '--with-jpeg-dir=/usr' '--with-freetype-dir=/usr' '--enable-gd-native-ttf' '--disable-debug' '--disable-rpath' '--enable-simplexml' '--enable-libxml' '--enable-filter' '--enable-ftp' '--enable-xmlreader=shared' '--enable-xmlwriter=shared' '--enable-hash' '--enable-phar=shared' '--enable-json=shared' '--enable-dom=shared' '--enable-pdo=shared' '--with-pdo-mysql=shared,/usr' '--with-pdo-sqlite=shared,/usr' '--enable-zip=shared' '--with-zlib-dir=/usr' '--enable-dba=shared' '--with-gdbm' '--with-db4'  '--with-tcadb' '--enable-xml' '--with-xsl=shared,/usr' '--enable-mbstring=shared,all' '--enable-ftp=shared' '--enable-bcmath=shared' '--enable-shmop=shared' '--enable-sysvsem=shared' '--enable-sysvshm=shared' '--enable-sysvmsg=shared' '--enable-sockets=shared' '--enable-exif=shared' '--enable-soap=shared' '--enable-wddx=shared' '--with-tidy=shared,/usr' '--with-pic' '--enable-inline-optimization' '--with-openssl' '--with-gettext=shared' '--with-config-file-scan-dir=/usr/local/lib/php.d/' '--enable-fpm' '--enable-cli' '--enable-pcntl' && \
    make && \
    make install && \
    cd /data1/download && \
    rm -Rf /data1/download/php-7.*


# 安装Redis/imagick/yaf/yar/yac/yaconf/xdebug
RUN pecl channel-update pecl.php.net && pecl install msgpack-2.0.2 yaf-3.0.5 yac-2.0.2 yaconf xdebug igbinary && rm -Rf /tmp/pear

# 安装Memcached、Imagick、YAR、YAC、PHPUNIT
RUN \
    # memcached
    cd /data1/download && \
    curl -o /data1/download/memcached-3.0.3.tgz https://pecl.php.net/get/memcached-3.0.3.tgz && \
    tar zxvf memcached-*.tgz && \
    cd  /data1/download/memcached-* && \
    phpize && \
    ./configure --enable-memcached-igbinary --enable-memcached-json --enable-memcached-msgpack && \
    make && \
    make install && \
    cd /data1/download && \
    rm -Rf /data1/download/memcached-* && \

    # Redis
    curl -o /data1/download/redis-3.1.3.tgz https://pecl.php.net/get/redis-3.1.3.tgz && \
    tar zxvf redis-*.tgz && \
    cd  /data1/download/redis-* && \
    phpize && \
    ./configure && \
    make && \
    make install && \
    cd /data1/download && \
    rm -Rf /data1/download/redis-* && \

    # Imagick 
    curl -o /data1/download/imagick-3.4.3.tgz http://pecl.php.net/get/imagick-3.4.3.tgz && \
    tar zxvf imagick-3.*.tgz && \
    cd  /data1/download/imagick-3.* && \
    phpize && \
    ./configure && \
    make && \
    make install && \
    cd /data1/download && \
    rm -Rf /data1/download/imagick-* && \
    \
    # Yar 
    curl -o /data1/download/yar-2.0.3.tgz http://pecl.php.net/get/yar-2.0.3.tgz && \
    tar zxvf yar-2.*.tgz && \
    cd  /data1/download/yar-2.* && phpize && \
    ./configure --enable-msgpack && \
    make && \
    make install && \
    cd /data1/download && \
    rm -Rf /data1/download/yar-* && \
    \
    # Swoole
    curl -o /data1/download/swoole-1.9.17.tgz http://pecl.php.net/get/swoole-1.9.17.tgz && \
    tar zxvf swoole-*.tgz && \
    cd /data1/download/swoole-* && \
    phpize && \
    ./configure --enable-sockets --enable-thread --enable-async-redis --enable-openssl --enable-timewheel && \
    make && \
    make install && \
    rm -Rf /data1/download/swoole-* && \

    # PHPUnit 
    curl -L -o /usr/local/bin/phpunit https://phar.phpunit.de/phpunit.phar && \
    chmod +x /usr/local/bin/phpunit

# 设置配置文件
ADD ./files/local/ /usr/local/

# 工作目录
WORKDIR /data1

# 初始化脚本
CMD ["/usr/local/sbin/php-fpm"]

