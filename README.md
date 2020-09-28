# Harbor #

This harbor provides docker configuration for your project. It is based on vessel by Fideloper LLC - https://vessel.shippingdocker.com/. It consists of:
 
* nginx container, 
* php container, 
* db container, 
* testing db container,
* node container,
* redis container

This script handles the current instance. To create new, install or update harbor, use harbor installer.

## Upgrade guide ##

### From 2.0 to 3.0 ###

TODO

### From 1.0 to 2.0 ###

As the containers has been changed, you have to rebuild all the containers and create global volume.

`docker volume create composercache` to create global volume for composer cache

then run 

`harbor rebuild -i -v` which will remove all used images and volumes. All data will be lost, so if you need the volumes, please run the command without -v. It will also recreate new images.

## Customization ##

If you need to customize some harbor/docker settings, it is recommended to modify `docker-compose.override.yml` file for a docker compose overrides and `.env.harbor` for harbor settings. You can define which version of php, mariadb or postgres and node will harbor be using.

Currently supported versions are:

```
PHP 7.2
PHP 7.3
PHP 7.4

MariaDB 10.5

Postgress 9
Postgress 10
Postgress 11
Postgress 12

Node 8
Node 10
Node 12
```

## Databases ##

Currently, we support only Postgres and MariaDB. By default, Postgres is prepared. To change to MariaDB you have to modify `docker-compose.override.yml` file, the snippet is already there. Just uncomment required parts.

In file `.env.harbor` you have to change the config for DB. Comment out Postgres part and uncomment Mariadb part. Example:

```
# Postgres
#HARBOR_DB_CONNECTION=pgsql
#HARBOR_DB_PORT=5432
#HARBOR_DB_TESTING_PORT=5433
#HARBOR_DB_DATA_PATH=/var/lib/postgresql/data
#HARBOR_DB_USER=postgres
# Mysql/Mariadb
HARBOR_DB_CONNECTION=mysql
HARBOR_DB_PORT=3306
HARBOR_DB_TESTING_PORT=3307
HARBOR_DB_DATA_PATH=/usr/local/lib/mysql
HARBOR_DB_USER=mysql
```

## Let's init laravel / craftable ##

If you have an existing laravel / craftable project, and you have not initialized this project, run 

`harbor init laravel` or `harbor init craftable` 

which will setup some .env variables, install required packages and will run npm

## Starting a harbor (docker) ##

To start the docker environment use:

`harbor start`

This will start all the docker containers and set up network and volumes correctly. So now we can play around with empty db or php. Now you can point your browser to the http://localhost, and you should be able to see a default `/` route mapped to public/index.php.

## Rebuild a harbor (docker) ##

In case you change some env values for docker, or changes some docker file or other configuration, you should run

`harbor rebuild`

NOTE: To rebuild images use `-i|--images`, to destroy volumes use `-v|--volumes` 

## Daily usage ##

In this section you can find all commands supported by harbor:

#### Docker commands ####

`harbor start` will start all containers based on `docker-compose.yml` and `docker-compose.override.yml` files.

`harbor up` will start all containers based on `docker-compose.yml` and `docker-compose.override.yml` files, but not as a daemon.

`harbor stop` will stop and destroy all containers.

`harbor restart`will stop and destroy and then starts again all containers.

`harbor rebuild` will stop and destroy all containers then starts again all containers, build them if changes has been made. Can be used with -i to remove images, -v to remove volumes, or -d to remove test database if in folder and not in volume

#### PHP container commands ####

`harbor artisan` OR `harbor art` will pass all additional arguments to php container to php artisan command, e.g. `harbor art make:migration` goes to php docker container and call `php artisan make:migration`.

`harbor composer` OR `harbor comp` will pass all additional arguments to php container to composer command, e.g. `harbor comp require brackets/craftable` goes to php docker container and call `composer require brackets/craftable`.

`harbor test` will run phpunit tests on new php container. All additional arguments are passed to phpunit.

#### NPM container commands ####

`harbor npm` will run npm command on node container and pass all additional arguments to npm.

Support for `yarn` and `gulp` has been dropped.

#### PGSQL container commands ####

`harbor db` will run `psql`/`mysql` command on db container with user and host form .env file and pass all additional arguments to `psql`/`mysql` command.

`harbor dump` will run `pg_dump`/`mysqldump` command on db container with user and host form .env file and pass all additional arguments to `pg_dump`/`mysqldump` command.

#### Global commands ####

`harbor ssh {container}` will connect to container and run bash in it, cannot be used with node. You can provide a second parameter `root` to open as root. 

`harbor exec` is an alias for docker-compose exec 

`harbor run` is an alias for docker-compose run 

## Install command (use with care) ##

`harbor new laravel` will install laravel application to current folder. See harbor installer, it is the recommended way.

`harbor new craftable` will install craftable application to current folder. See harbor installer, it is the recommended way.

## SSH keys ##

To use ssh keys in php container, copy your keys to `./harbor/ssh`. You have to restart container after adding keys. SSH keys may be required for some git repositories.