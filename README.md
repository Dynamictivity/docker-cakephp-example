docker-cakephp-example
==============

Just a little Docker POC in order to have a complete stack for running CakePHP into Docker containers using docker-compose tool.

This example takes it a step further by hosting the source-code in a standalone container, to make infrastructure upgrades seamless. You can find the source-code for the example application here: https://github.com/Dynamictivity/cakephp-bookmarker

# Installation

First, clone this repository:

```bash
$ git clone git@github.com:Dynamictivity/docker-cakephp.git
```

Change into the working directory that you just cloned and run:

```bash
$ docker-compose up
```

You are done, you can visit your CakePHP application on the following URL: `http://localhost`

## Database Migrations and Seeds

When the container spins up it runs the following 2 commands (aside from `composer install`):

```bash
$ cd /www; bin/cake migrations migrate
$ cd /www; bin/cake migrations seed --seed $DB_SEED
```

You can specify the database seed file inside of `docker-compose.yml` by changing the `DB_SEED:` value to that of your database seed file.

# Vagrant
You can also use `vagrant` for testing by typing the following command from the work tree: `vagrant up`

Run the following commands:

```bash
$ cd /vagrant
$ docker-compose up
```

# How it works?

Here are the `docker-compose` images:

* `db`: This is the MySQL database container (can be changed to postgresql or whatever in `docker-compose.yml` file),
* `php`: This is the PHP-FPM container including the application volume mounted on,
* `nginx`: This is the Nginx webserver container in which php volumes are mounted too.

This results in the following running containers:

```bash
> $ docker-compose ps
        Name                      Command               State              Ports
        -------------------------------------------------------------------------------------------
        cakephp-bookmarker_1   /sbin/my_init                 Up
        db_1                   docker-entrypoint.sh mysqld   Up      0.0.0.0:3306->3306/tcp
        nginx_1                nginx                         Up      443/tcp, 0.0.0.0:80->80/tcp
        php_1                  /init.sh                      Up      9000/tcp
```

# Code license

You are free to use the code in this repository under the terms of the 0-clause BSD license. LICENSE contains a copy of this license.
