# PHP-SQLSRV

## About
This image use [Docker Compose](https://docs.docker.com/compose/) for work.

## Softs

In this image there is installed some softwares to help your development, like Git, cURL, Apache, Composer and ODBC Driver.

### Tags and softs versions

PHP 7.0.

## Run Dockerfile

~~~
$ docker-compose up --build
~~~

## Composer Install

By default, Composer runs as root inside the container. This can lead to permission issues on your host filesystem. You can run Composer as your local user using this command:

~~~
$ docker-compose run --user $(id -u):$(id -g) webserver composer install
~~~

And go to [localhost](http://localhost:8080).
