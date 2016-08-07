docker-cakephp-example
==============

Just a little Docker POC in order to have a complete stack for running CakePHP into Docker containers using docker-compose tool.

This example takes it a step further by hosting the source-code in a standalone container, to make infrastructure upgrades seamless. You can find the source-code for the example application here: https://github.com/Dynamictivity/cakephp-bookmarker

### Compatibility
- CakePHP3

# Installation

First, clone this repository:

```bash
$ git clone git@github.com:Dynamictivity/docker-cakephp-example.git
```

Change into the working directory that you just cloned and run:

```bash
$ docker-compose up
```

You are done, you can visit your CakePHP application on the following URL: `http://localhost`

# Custom Application Configuration

To use your own custom application you just need to build your CakePHP application into a standalone docker image by creating a `Dockerfile` inside of the root of your CakePHP application. We recommend `phusion/baseimage` -- example dockerfile below:

```
# Use phusion/baseimage as base image. To make your builds reproducible, make
# sure you lock down to a specific version, not to `latest`!
# See https://github.com/phusion/baseimage-docker/blob/master/Changelog.md for
# a list of version numbers.
FROM phusion/baseimage:0.9.19

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]

ADD . /www

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
```

Once you have built your CakePHP docker image and uploaded it to a registry, simply change the php image portion of the `docker-compose.yml` -- replace `cakephp-bookmarker` below, with your custom docker image as follows:

```
cakephp:
    image: travisrowland/cakephp-bookmarker:latest
```

## E-Mail Configuration
Change the following variables in `docker-compose.yml` to configure email in your application:

```
EMAIL_HOST: 'localhost'
EMAIL_PORT: '25'
EMAIL_TIMEOUT: '30'
EMAIL_USERNAME: 'user'
EMAIL_PASSWORD: 'secret'
EMAIL_TLS:
EMAIL_TRANSPORT_DEFAULT_URL:
```

## Database Migrations and Seeds

When the container spins up it runs the following 2 commands (aside from `composer install`):

```bash
$ cd /www; bin/cake migrations migrate
$ cd /www; bin/cake migrations seed --seed $DB_SEED
```

You can specify the database seed file inside of `docker-compose.yml` by changing the `DB_SEED:` value to that of your database seed file.

# Vagrant
You can also use `vagrant` for testing by typing the following command from the work tree: `vagrant up`

Once you are logged into the Vagrant box run the following commands:

```bash
$ cd /vagrant
$ docker-compose up
```

# How it works?

Here are the `docker-compose` running containers:

* `cakephp`: This is the stand-alone application source-code container which contains your CakePHP application
* `db`: This is the MySQL database container including the application volume mounted from cakephp
* `nginx`: This is the Nginx webserver container in which cakephp volumes are mounted to
* `php`: This is the PHP-FPM container including the application volume mounted from cakephp

This results in the following running containers:

```bash
> $ docker-compose ps
        Name                      Command               State              Ports
        -------------------------------------------------------------------------------------------
        cakephp_1              /sbin/my_init                 Up
        db_1                   docker-entrypoint.sh mysqld   Up      0.0.0.0:3306->3306/tcp
        nginx_1                nginx                         Up      443/tcp, 0.0.0.0:80->80/tcp
        php_1                  /init.sh                      Up      9000/tcp
```

# Code license

You are free to use the code in this repository under the terms of the 0-clause BSD license. LICENSE contains a copy of this license.
