FROM ubuntu:16.04
MAINTAINER grass

# Install base packages

RUN cat /etc/apt/sources.list
RUN sed -i 's/http:\/\/archive.ubuntu.com\/ubuntu\//http:\/\/mirrors.sohu.com\/ubuntu\//' /etc/apt/sources.list

RUN DEBIAN_FRONTEND=noninteractive apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install python-software-properties
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install software-properties-common
RUN DEBIAN_FRONTEND=noninteractive LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php

RUN DEBIAN_FRONTEND=noninteractive apt-get update

RUN DEBIAN_FRONTEND=noninteractive apt-get -y install \
	curl \
	vim \
	apache2 \
	php7.1 \
	libapache2-mod-php7.1

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y \
    php7.1-cli \
    php7.1-common \
    php7.1-dev \
    php7.1-json \
    php7.1-opcache \
    php7.1-mysql \
    php7.1-phpdbg \
    php7.1-mbstring \
    php7.1-gd \
    php7.1-imap \
    php7.1-ldap \
    php7.1-pgsql \
    php7.1-sqlite \
    php7.1-pdo \
    php7.1-pspell \
    php7.1-recode \
    php7.1-tidy \
    php7.1-intl \
    php7.1-curl \
    php7.1-zip \
    php7.1-xml \
    php-mongodb \
    php-redis \
    php-memcache \
    php-xdebug

# Clean apt cache
RUN apt-get clean -y && \
    apt-get autoclean -y && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /var/cache/apt/archives/*

RUN phpenmod mcrypt
RUN a2enmod rewrite
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf && \
    sed -i "s/variables_order.*/variables_order = \"EGPCS\"/g" /etc/php/7.1/apache2/php.ini

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
