FROM ubuntu:16.04
MAINTAINER grass

# Install base packages

RUN sed -i 's/http:\/\/archive.ubuntu.com\/ubuntu\//http:\/\/mirrors.sohu.com\/ubuntu\//' /etc/apt/sources.list

RUN apt-get update

RUN apt-get -y install \
	apache2 \
	php7.0 \
	libapache2-mod-php7.0

RUN apt-get -y install \
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
	php7.0-imap \
	php7.0-interbase \
	php7.0-intl \
	php7.0-mbstring \
	php7.0-mcrypt \
	php7.0-phpdbg \
	php7.0-soap \
	php7.0-sybase \
	php7.0-xsl \
	php7.0-zip

# Clean apt cache
RUN apt-get clean -y && \
    apt-get autoclean -y && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /var/cache/apt/archives/*

RUN phpenmod mcrypt
RUN a2enmod rewrite
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf && \
    sed -i "s/variables_order.*/variables_order = \"EGPCS\"/g" /etc/php/7.0/apache2/php.ini

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
