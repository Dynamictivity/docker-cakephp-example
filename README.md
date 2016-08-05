docker-cakephp
==============

Just a little Docker POC in order to have a complete stack for running CakePHP into Docker containers using docker-compose tool.

# Installation

First, clone this repository:

```bash
$ git clone git@github.com:Dynamictivity/docker-cakephp.git
```

Next, edit the `docker-compose.yml` file and change the `REPO:` value to the URL of your application's GIT repository.

Finally (required only for SSH GIT repositories), edit `php-fpm/id_rsa` file and put your GIT deployment (private key) in there so that the docker container can access your private GIT repository. If you are using GIT via SSH you'll also want to change the `REPO_HOST:` value to the FQDN of your GIT server host, that way the host key can be automatically accepted.

Then, run:

```bash
$ docker-compose up
```

You are done, you can visit your CakePHP application on the following URL: `http://localhost`

_Note :_ you can rebuild all Docker images by running:

```bash
$ docker-compose build
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

Run the following commands:

```bash
$ cd /vagrant
$ docker-compose up
```

# How it works?

Here are the `docker-compose` built images:

* `db`: This is the MySQL database container (can be changed to postgresql or whatever in `docker-compose.yml` file),
* `php`: This is the PHP-FPM container including the application volume mounted on,
* `nginx`: This is the Nginx webserver container in which php volumes are mounted too.

This results in the following running containers:

```bash
> $ docker-compose ps
        Name                      Command               State              Ports
        -------------------------------------------------------------------------------------------
        docker_db_1            /entrypoint.sh mysqld            Up      0.0.0.0:3306->3306/tcp
        docker_nginx_1         nginx                            Up      443/tcp, 0.0.0.0:80->80/tcp
        docker_php_1           php5-fpm -F                      Up      9000/tcp
```

# Read logs

You can access Nginx and CakePHP application logs in the following directories on your host machine:

* `logs/nginx`
* `logs/cakephp`

# Code license

You are free to use the code in this repository under the terms of the 0-clause BSD license. LICENSE contains a copy of this license.
