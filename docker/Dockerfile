FROM centos:centos7
ENV container docker
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;
VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]

RUN yum install -y epel-release && yum clean all

RUN rpm -ivh http://ftp.riken.jp/Linux/remi/enterprise/remi-release-7.rpm

RUN yum -y update && yum clean all

RUN yum -y install httpd && yum clean all

# RUN yum update -y && yum clean all

# # Repository
# # EPEL
# RUN yum install -y epel-release
# # Install apache
# RUN yum -y install httpd
COPY ./vhost.conf /etc/httpd/conf.d/vhost.conf

# # remi
# RUN rpm -ivh http://ftp.riken.jp/Linux/remi/enterprise/remi-release-7.rpm

# Install PHP
RUN yum install -y \
    --enablerepo=remi,remi-php80 \
    php php-devel php-mbstring php-pdo php-xml php-gd php-fpm php-mysqlnd php-opcache php-pecl-zip libzip5

# PHP の設定
COPY ./php.ini /usr/local/etc/php/

# # apache の設定
# ENV APACHE_DOCUMENT_ROOT /var/www/html/evms/public
# RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
# RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf
# RUN cd /etc/apache2/mods-enabled \
#     && ln -s ../mods-available/rewrite.load

# # 必要なパッケージをインストール
# RUN apt-get update \
#   && apt-get install -y \
#     libpq-dev \
#     libpng-dev \
#     libzip-dev \
#     libfreetype6-dev \
#     libjpeg-dev \
#     libmcrypt-dev \
#     libonig-dev \
#     gcc \
#     make \
#     less \
#     vim \
#     sqlite3 \
#     libsqlite3-dev

# # PHP エクステンションをインストール
# RUN docker-php-ext-install \
#     zip \
#     gd \
#     bcmath \
#     pdo \
#     pdo_sqlite \
#     mbstring

# GD ライブラリに関する設定
# RUN docker-php-ext-configure \
#     gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
#     && docker-php-ext-install -j$(nproc) gd

# composer2.x をインストール https://hub.docker.com/_/composer
COPY --from=composer /usr/bin/composer /usr/bin/composer

RUN systemctl enable httpd.service
EXPOSE 80
