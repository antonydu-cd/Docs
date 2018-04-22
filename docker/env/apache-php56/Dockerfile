FROM ubuntu:16.04
MAINTAINER grass

# Install base packages
RUN sed -i 's/http:\/\/archive.ubuntu.com\/ubuntu\//http:\/\/mirrors.sohu.com\/ubuntu\//' /etc/apt/sources.list

RUN apt-get update

RUN export LANG=C.UTF-8 && \
    apt-get install -y software-properties-common && \
    add-apt-repository -y ppa:nginx/stable && \
    LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php -y && \
    apt-get update

# Install Apache
RUN apt-get install -y apache2

# Install PHP
RUN apt-get -y install \
  curl \
  vim \
  php5.6 \
  libapache2-mod-php5.6

RUN apt-get -y install \
  php5.6-cgi \
  php5.6-cli \
  php5.6-common \
  php5.6-curl \
  php5.6-dev \
  php5.6-gd \
  php5.6-gmp \
  php5.6-json \
  php5.6-ldap \
  php5.6-mysql \
  php5.6-odbc \
  php5.6-pgsql \
  php5.6-pspell \
  php5.6-readline \
  php5.6-recode \
  php5.6-snmp \
  php5.6-tidy \
  php5.6-xmlrpc \
  php5.6-enchant \
  php5.6-fpm \
  php5.6-imap \
  php5.6-interbase \
  php5.6-intl \
  php5.6-mcrypt \
  php5.6-sybase \
  php5.6-xsl \
  php5.6-mbstring \
  php5.6-xdebug \
  php5.6-soap

# Clean apt cache
RUN apt-get clean -y && \
    apt-get autoclean -y && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /var/cache/apt/archives/*

RUN phpenmod mcrypt
RUN a2enmod rewrite
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf && \
    sed -i "s/variables_order.*/variables_order = \"EGPCS\"/g" /etc/php/5.6/apache2/php.ini

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
