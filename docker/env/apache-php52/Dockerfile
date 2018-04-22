FROM ubuntu:14.04
MAINTAINER Grass

# Install base packages
#RUN sed -i 's/http:\/\/archive.ubuntu.com\/ubuntu\//http:\/\/mirrors.sohu.com\/ubuntu\//' /etc/apt/sources.list
RUN echo "deb http://archive.ubuntu.com/ubuntu precise main restricted universe\ndeb http://archive.ubuntu.com/ubuntu precise-updates main restricted universe\ndeb http://security.ubuntu.com/ubuntu precise-security main restricted universe multiverse\n" >> /etc/apt/sources.list
# Update sources
RUN apt-get update

# Remove php+
RUN apt-get remove \
  apache2 \
  apache2-bin \
  apache2-data \
  apache2-mpm-prefork \
  libapache2-mod-php5 \
  dh-php5 \
  php-pear \
  php5 \
  php5-cgi \
  php5-cli \
  php5-common \
  php5-curl \
  php5-dev \
  php5-gd \
  php5-json \
  php5-mcrypt \
  php5-mysql \
  php5-readline \
  pkg-php-tools

# Install Apache
RUN apt-get install -y \
apache2-mpm-prefork=2.2.22-1ubuntu1.11 \
apache2-prefork-dev=2.2.22-1ubuntu1.11 \
apache2.2-bin=2.2.22-1ubuntu1.11 \
apache2.2-common=2.2.22-1ubuntu1.11 \
apache2-utils=2.2.22-1ubuntu1.11

# Install depends
RUN apt-get install -y gcc \
    make \
    build-essential \
    libxml2-dev \
    libcurl4-openssl-dev \
    libpcre3-dev \
    libbz2-dev \
    libjpeg-dev \
    libpng12-dev \
    libfreetype6-dev \
    libt1-dev \
    libmcrypt-dev \
    libmhash-dev \
    freetds-dev \
    libmysqlclient-dev \
    unixodbc-dev \
    libxslt1-dev

RUN mkdir -pv /usr/include/freetype2/freetype
RUN ln -sf /usr/include /opt/include && ln -s /usr/lib64 /opt/lib
RUN ln -sf /usr/include/freetype2/freetype.h /usr/include/freetype2/freetype/freetype.h

RUN mkdir -pv /php/php-5.2.17
ADD php-5.2.17 /php/php-5.2.17
#RUN cd /php && wget -c -t 3 -O ./php-5.2.17.tar.gz http://museum.php.net/php5/php-5.2.17.tar.gz && tar xvfz php-5.2.17.tar.gz
#RUN cd /php/php-5.2.17 && wget -c -t 3 -O ./libxml29_compat.patch https://mail.gnome.org/archives/xml/2012-August/txtbgxGXAvz4N.txt
#RUN cd /php/php-5.2.17 && patch -p0 -b < libxml29_compat.patch

#RUN cd /php/php-5.2.17 && wget -c -t 3 -O ./debian_patches_disable_SSLv2_for_openssl_1_0_0.patch https://bugs.php.net/patch-display.php\?bug_id\=54736\&patch\=debian_patches_disable_SSLv2_for_openssl_1_0_0.patch\&revision=1305414559\&download\=1
#RUN cd /php/php-5.2.17 && patch -p1 -b < debian_patches_disable_SSLv2_for_openssl_1_0_0.patch

#RUN cd /php/php-5.2.17 && wget -c -t 3 -O - http://php-fpm.org/downloads/php-5.2.17-fpm-0.5.14.diff.gz | gunzip > ./php-5.2.17-fpm-0.5.14.patch
#RUN cd /php/php-5.2.17 && patch -p1 < php-5.2.17-fpm-0.5.14.patch

# Gen config
RUN cd /php/php-5.2.17 && ./configure \
    --prefix=/usr/share/php52 \
    --datadir=/usr/share/php52 \
    --mandir=/usr/share/man \
    --bindir=/usr/bin \
    --with-libdir=lib/x86_64-linux-gnu \
    --includedir=/usr/include \
    --with-config-file-path=/etc/php \
    --with-config-file-scan-dir=/etc/php/conf.d \
    --with-apxs2=/usr/bin/apxs2 \
    --disable-debug \
    --with-regex=php \
    --disable-rpath \
    --disable-static \
    --disable-posix \
    --with-pic \
    --with-layout=GNU \
    --with-pear=/usr/share/php \
    --enable-calendar \
    --enable-sysvsem \
    --enable-sysvshm \
    --enable-sysvmsg \
    --enable-bcmath \
    --with-bz2 \
    --enable-ctype \
    --without-gdbm \
    --with-iconv \
    --enable-exif \
    --enable-ftp \
    --enable-cli \
    --with-gettext \
    --enable-mbstring \
    --with-pcre-regex \
    --enable-shmop \
    --enable-sockets \
    --enable-wddx \
    --with-mcrypt \
    --with-zlib \
    --enable-pdo \
    --with-curl \
    --enable-inline-optimization \
    --enable-xml \
    --enable-pcntl \
    --enable-mbregex \
    --with-mhash \
    --with-xsl \
    --enable-zip \
    --with-gd \
    --with-pdo-mysql \
    --with-jpeg-dir=/usr/lib \
    --with-png-dir=/usr/lib \
    --with-mysql \
    --with-openssl \
    --with-mysqli \
    --with-kerberos \
    --enable-dbase \
    --with-mysqli=/usr/bin/mysql_config \
    --enable-gd-native-ttf \
    --with-t1lib=/usr \
    --with-freetype-dir=/usr \
    --with-ldap \
    --with-kerberos=/usr \
    --with-unixODBC=shared,/usr \
    --with-imap-ssl \
    --with-mssql \
    --without-sqlite \
    --with-sqlite \
    --without-pdo-sqlite \
    --enable-soap \
    --with-pdo-sqlite

# Install
RUN cd /php/php-5.2.17 && make && make install

# Clean apt cache
RUN apt-get clean -y && \
    apt-get autoclean -y && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /var/cache/apt/archives/*

RUN a2enmod rewrite
RUN cd /php/php-5.2.17 && mkdir -pv /etc/php/ && cp php.ini-recommended /etc/php/php.ini && rm -rf /php
RUN echo "ServerName localhost\nAddType application/x-httpd-php  .php\nDirectoryIndex index.php index.html" >> /etc/apache2/apache2.conf && \
    sed -i "s/variables_order.*/variables_order = \"EGPCS\"/g" /etc/php/php.ini
ENV ALLOW_OVERRIDE **False**

# Add vhosts file
RUN rm /etc/apache2/sites-enabled/* -Rf
ADD app.conf /etc/apache2/sites-enabled/app.conf

# Add image configuration and scripts
ADD run.sh /run.sh
RUN chmod 755 /*.sh

# Configure /app folder with sample app
RUN mkdir -p /app && rm -fr /var/www/html && ln -s /app /var/www/html

ADD app/ /app

EXPOSE 80

WORKDIR /app

CMD ["/run.sh"]
