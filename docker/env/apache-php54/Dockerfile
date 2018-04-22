FROM ubuntu:14.04
MAINTAINER Grass

# Install base packages
RUN sed -i 's/http:\/\/archive.ubuntu.com\/ubuntu\//http:\/\/mirrors.sohu.com\/ubuntu\//' /etc/apt/sources.list

RUN apt-get update

# Install Apache
RUN apt-get install -y apache2 apache2-dev

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

RUN mkdir -pv /php/php-5.4.45
ADD php-5.4.45 /php/php-5.4.45

# Gen config
RUN cd /php/php-5.4.45 && ./configure \
    --prefix=/usr/share/php54 \
    --datadir=/usr/share/php54 \
    --mandir=/usr/share/man \
    --bindir=/usr/bin \
    --with-libdir=lib/x86_64-linux-gnu \
    --includedir=/usr/include \
    --with-config-file-path=/etc/php \
    --with-config-file-scan-dir=/etc/php/conf.d \
    --with-apxs2=/usr/bin/apxs \
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
    --with-mysqli=/usr/bin/mysql_config \
    --enable-gd-native-ttf \
    --with-t1lib=/usr \
    --with-freetype-dir=/usr \
    --with-ldap \
    --with-kerberos=/usr \
    --with-unixODBC=shared,/usr \
    --with-imap-ssl \
    --with-mssql \
    --without-pdo-sqlite \
    --enable-soap \
    --with-pdo-sqlite

# Install
RUN cd /php/php-5.4.45 && make && make install

# Clean apt cache
RUN apt-get clean -y && \
    apt-get autoclean -y && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /var/cache/apt/archives/*

RUN a2enmod rewrite
RUN cd /php/php-5.4.45 && mkdir -pv /etc/php/ && cp php.ini-development /etc/php/php.ini && rm -rf /php
RUN echo "ServerName localhost\nAddType application/x-httpd-php  .php\nDirectoryIndex index.php index.html" >> /etc/apache2/apache2.conf && \
    sed -i "s/variables_order.*/variables_order = \"EGPCS\"/g" /etc/php/php.ini
RUN cd /etc/apache2/mods-enabled && \
rm mpm_event.load mpm_event.conf && \
ln -s ../mods-available/mpm_prefork.conf mpm_prefork.conf && \
ln -s ../mods-available/mpm_prefork.load mpm_prefork.load
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
