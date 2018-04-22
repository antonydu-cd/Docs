FROM ubuntu:14.04
MAINTAINER Grass

# Add sources
RUN echo "deb http://archive.ubuntu.com/ubuntu precise main restricted universe\ndeb http://archive.ubuntu.com/ubuntu precise-updates main restricted universe\ndeb http://security.ubuntu.com/ubuntu precise-security main restricted universe multiverse\n" >> /etc/apt/sources.list

# Uodate
RUN apt-get update

# Clear all php/apache2
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

# Install php & apache
RUN apt-get install -y \
  apache2-mpm-prefork=2.2.22-1ubuntu1.11 \
  apache2-prefork-dev=2.2.22-1ubuntu1.11 \
  apache2.2-bin=2.2.22-1ubuntu1.11 \
  apache2.2-common=2.2.22-1ubuntu1.11 \
  apache2-utils=2.2.22-1ubuntu1.11 \
  libapache2-mod-php5=5.3.10-1ubuntu3.26 \
  php5-common=5.3.10-1ubuntu3.26 \
  php5-cli=5.3.10-1ubuntu3.26 \
  php5-curl=5.3.10-1ubuntu3.26 \
  php5-dev=5.3.10-1ubuntu3.26 \
  php5-gd=5.3.10-1ubuntu3.26 \
  php5-mcrypt=5.3.5-0ubuntu1 \
  php5-mysql=5.3.10-1ubuntu3.26

# Clean apt cache
RUN apt-get clean -y && \
    apt-get autoclean -y && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /var/cache/apt/archives/*

# Enable rewrite module
RUN a2enmod rewrite

# Configure
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf && \
    sed -i "s/variables_order.*/variables_order = \"EGPCS\"/g" /etc/php5/apache2/php.ini
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
