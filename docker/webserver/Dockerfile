FROM ubuntu:16.04

# env variable for the project root dir
ENV PROJECT_ROOT /var/www/html
ENV DEBIAN_FRONTEND=noninteractive

LABEL maintainer="Leandro Ponce (leandro.ponce@improvein.com/ponce.leandro@outlook.com)"

# update package list
RUN apt-get update

# install curl and git
RUN apt-get install -y curl git

# install apache
RUN apt-get install -y apache2

# install php
RUN apt-get -y install php7.0 mcrypt php7.0-mcrypt php-mbstring php-pear php7.0-dev php7.0-xml php7.0-zip
RUN apt-get install -y libapache2-mod-php7.0

# install pre requisites
RUN apt-get update
RUN apt-get install -y apt-transport-https
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
RUN curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
RUN apt-get update
RUN ACCEPT_EULA=Y apt-get install -y msodbcsql mssql-tools
RUN apt-get install -y unixodbc-utf16
RUN apt-get install -y unixodbc-dev-utf16

# install driver sqlsrv
RUN pecl install sqlsrv
RUN pecl install pdo_sqlsrv

# load driver sqlsrv
RUN echo "extension=pdo_sqlsrv.so" >> /etc/php/7.0/apache2/conf.d/30-pdo_sqlsrv.ini
RUN echo "extension=sqlsrv.so" >> /etc/php/7.0/apache2/conf.d/20-sqlsrv.ini
RUN echo "extension=pdo_sqlsrv.so" >> /etc/php/7.0/cli/conf.d/30-pdo_sqlsrv.ini
RUN echo "extension=sqlsrv.so" >> /etc/php/7.0/cli/conf.d/20-sqlsrv.ini

# install ODBC Driver
RUN export DEBIAN_FRONTEND=noninteractive && apt-get update && ACCEPT_EULA=Y apt-get install -y msodbcsql mssql-tools unixodbc-dev
RUN echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
RUN echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
RUN exec bash

# install locales
RUN apt-get install -y locales && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && locale-gen

# custom configurations
COPY apache2.conf /etc/apache2/apache2.conf

# permissions on app directories
RUN usermod -u 1000 www-data

# install composer
COPY --from=composer:1.5 /usr/bin/composer /usr/bin/composer

WORKDIR $PROJECT_ROOT

EXPOSE 80

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]