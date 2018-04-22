FROM ubuntu:16.04
MAINTAINER Grass

# Replace sources
RUN sed -i 's/http:\/\/archive.ubuntu.com\/ubuntu\//http:\/\/mirrors.sohu.com\/ubuntu\//' /etc/apt/sources.list

# Keep upstart from complaining
RUN dpkg-divert --local --rename --add /sbin/initctl
RUN ln -sf /bin/true /sbin/initctl

# Let the conatiner know that there is no tty
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update

# Basic Requirements
RUN apt-get -y install \
  snmp \
  pwgen \
  python-setuptools

# Requirements
RUN apt-get -y install \
  nginx \
  php7.0-cgi \
  php7.0-cli \
  php7.0-common \
  php7.0-curl \
  php7.0-dev \
  php7.0-gd \
  php7.0-gmp \
  php7.0-json \
  php7.0-ldap \
  php7.0-mysql \
  php7.0-odbc \
  php7.0-opcache \
  php7.0-pgsql \
  php7.0-pspell \
  php7.0-readline \
  php7.0-recode \
  php7.0-snmp \
  php7.0-sqlite3 \
  php7.0-tidy \
  php7.0-xml \
  php7.0-xmlrpc \
  php7.0-bcmath \
  php7.0-bz2 \
  php7.0-dba \
  php7.0-enchant \
  php7.0-fpm \
  php7.0-imap \
  php7.0-interbase \
  php7.0-intl \
  php7.0-mbstring \
  php7.0-mcrypt \
  php7.0-phpdbg \
  php7.0-soap \
  php7.0-sybase \
  php7.0-xsl \
  php7.0-zip \
  php-xdebug

# Clean apt cache
RUN apt-get clean -y && \
    apt-get autoclean -y && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /var/cache/apt/archives/*

# Nginx config
RUN sed -i -e"s/keepalive_timeout\s*65/keepalive_timeout 2/" /etc/nginx/nginx.conf
RUN sed -i -e"s/keepalive_timeout 2/keepalive_timeout 2;\n\tclient_max_body_size 100m/" /etc/nginx/nginx.conf
RUN echo "daemon off;" >> /etc/nginx/nginx.conf

# php-fpm config
RUN sed -i -e "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g" /etc/php/7.0/fpm/php.ini
RUN sed -i -e "s/upload_max_filesize\s*=\s*2M/upload_max_filesize = 100M/g" /etc/php/7.0/fpm/php.ini
RUN sed -i -e "s/post_max_size\s*=\s*8M/post_max_size = 100M/g" /etc/php/7.0/fpm/php.ini
RUN sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /etc/php/7.0/fpm/php-fpm.conf
RUN sed -i -e "s/;catch_workers_output\s*=\s*yes/catch_workers_output = yes/g" /etc/php/7.0/fpm/pool.d/www.conf
RUN find /etc/php/7.0/cli/conf.d/ -name "*.ini" -exec sed -i -re 's/^(\s*)#(.*)/\1;\2/g' {} \;

# Nginx site conf
ADD ./nginx-site.conf /etc/nginx/sites-available/vhost.conf
RUN rm  -f /etc/nginx/sites-enabled/default && ln -sf /etc/nginx/sites-available/vhost.conf /etc/nginx/sites-enabled/default

# PHP log conf
ADD ./www.conf /etc/php/7.0/fpm/pool.d/www.conf
RUN echo '' > /var/log/fpm-php.www.log

# Supervisor Config
RUN /usr/bin/easy_install supervisor
RUN /usr/bin/easy_install supervisor-stdout
ADD ./supervisord.conf /etc/supervisord.conf

# Document root
RUN mkdir /app -p && \
        mkdir /run/php -p && \
        rm -rf /usr/share/nginx/www && \
        ln -sf /app /usr/share/nginx/www && \
        chown -R www-data:www-data /usr/share/nginx/www

# Demo
ADD ./app /app

# Startup script
ADD ./start.sh /start.sh
RUN chmod 755 /start.sh

# Private expose
EXPOSE 80

# Work dir
WORKDIR /app

# Command line
CMD ["/bin/bash", "/start.sh"]
