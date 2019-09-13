# Harbor #

This harbor provides docker configuration for your project. It is based on vessel by Fideloper LLC - https://vessel.shippingdocker.com/. It consist of
 
* nginx container, 
* php container, 
* postgres container, 
* testing postgres container,
* node container,
* redis container

This script handles the current instance. To create new, install or update harbor, use harbor installer.

## Upgrade guide ##

### From 1.0 to 2.0 ###

As the containers has been changed, you have to rebuild all the containers and create global volume.

`docker volume create composercache` to create global volume for composer cache

then run 

`harbor rebuild -i -v` which will remove all used images and volumes. All data will be lost, so if you need the volumes, please run the command without -v. It will also recreate new images. 

## Let's init laravel / craftable ##

If you have an existing laravel / craftable project and you have not initialize this project, run 

`harbor init`

which will setup some .env variables, install required packages and will run npm

## Starting a harbor (docker) ##

To start the docker environment use:

`harbor start`

This will start all the docker containers and set up network and volumes correctly. So now we can play around with empty PostgreSQL or php. Now you can point your browser to the http://localhost and you should be able to see a default `/` route mapped to public/index.php.

## Rebuild a harbor (docker) ##

In case you change some env values for docker, or changes some docker file or other configuration, you should run

`harbor rebuild`

NOTE: To rebuild images use `-i|--images`, to destroy volumes use `-v|--volumes` 

## Daily usage ##

In this section you can find all commands supported by harbor:

#### Docker commands ####

`harbor start` will start all containers based on docker-compose.yml file.

`harbor up` will start all containers based on docker-compose.yml file, but not as a daemon.

`harbor stop` will stop and destroy all containers.

`harbor restart`will stop and destroy and then starts again all containers.

`harbor rebuild` will stop and destroy all containers then starts again all containers, build them if changes has been made. Can be used with -i to remove images, -v to remove volumes, or -d to remove test database if in folder and not in volume

#### PHP container commands ####

`harbor artisan` OR `harbor art` will pass all additional arguments to php container to php artisan command, e.g. `harbor art make:migration` goes to php docker container and call `php artisan make:migration`.

`harbor composer` OR `harbor comp` will pass all additional arguments to php container to composer command, e.g. `harbor comp require brackets/craftable` goes to php docker container and call `composer require brackets/craftable`.

`harbor test` will run phpunit tests on new php container. All additional arguments are passed to phpunit.

#### NPM container commands ####

`harbor npm` will run npm command on node container and pass all additional arguments to npm.

`harbor yarn` will run yarn command on node container and pass all additional arguments to yarn.

`harbor gulp` will run gulp command on node container from node modules and pass all additional arguments to gulp.

#### PGSQL container commands ####

`harbor psql` will run psql command on pgsql container with username and host form .env file and pass all additional arguments to psql command.

`harbor pg_dump` will run pg_dump command on pgsql container with username and host form .env file and pass all additional arguments to pg_dump command.

#### Global commands ####

`harbor ssh {container}` will connect to container and run bash in it, cannot be used with node. You can provide a second parameter `root` to open as root. 

`harbor exec` is an alias for docker-compose exec 

`harbor run` is an alias for docker-compose run 

## Install command (use with care) ##

`harbor new laravel` will install laravel application to current folder. See harbor installer, it is the recommended way.

`harbor new craftable` will install craftable application to current folder. See harbor installer, it is the recommended way.

## SSH keys ##

To use ssh keys in php container, copy your keys to ./docker/php/ssh. You have to restart container after adding keys. SSH keys may be required for some git repositories.