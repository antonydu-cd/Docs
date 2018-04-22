FROM ubuntu:14.04
MAINTAINER grass

# Install base packages

RUN cat /etc/apt/sources.list
RUN sed -i 's/http:\/\/archive.ubuntu.com\/ubuntu\//http:\/\/mirrors.sohu.com\/ubuntu\//' /etc/apt/sources.list

RUN apt-get update

RUN apt-get -y install \
	curl \
	vim \
	apache2 \
	php5 \
	libapache2-mod-php5

RUN apt-get -y install \
    php5-cgi \
	php5-cli \
	php5-common \
	php5-curl \
	php5-dev \
	php5-gd \
	php5-gmp \
	php5-json \
	php5-ldap \
	php5-mysql \
	php5-odbc \
	php5-pgsql \
	php5-pspell \
	php5-readline \
	php5-recode \
	php5-snmp \
	php5-tidy \
	php5-xmlrpc \
	php5-enchant \
	php5-fpm \
	php5-imap \
	php5-interbase \
	php5-intl \
	php5-mcrypt \
	php5-sybase \
	php5-xsl

# Clean apt cache
RUN apt-get clean -y && \
    apt-get autoclean -y && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /var/cache/apt/archives/*

RUN ln -s /etc/php5/mods-available/mcrypt.ini /etc/php5/apache2/conf.d/mcrypt.ini
RUN a2enmod rewrite
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
