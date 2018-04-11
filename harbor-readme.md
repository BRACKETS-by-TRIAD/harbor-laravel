# Adding Harbor #

In order to initialize harbor to the existing project or when starting a new project (in empty folder) use
`harbor add`

# Let's add some code #

A) If you have an existing Laravel/Craftable project and it does not matter if it has already been initialized (.env file exists, composer was installed, ...) or not, you just have to run

`harbor laravel init`

or

<!-- This will run:
`cp .env.example .env`
make some some changes to .env file (setting correct DB_HOST and use PostgreSQL over MySQL)
`habror composer require predis/predis`
`habror composer install`
`harbor artisan key:generate`
`harbor restart`
`harbor artisan migrate`
`harbor npm install`
`harbor npm run dev`-->

`harbor craftable init`

Both are alias for `harbor init`

B) If you want to create new Laravel project, you can use:

`harbor laravel new`

This will run:
1) laravel new
2) `harbor laravel init`

C) If you want to create new Craftable project, you can use:

`harbor craftable new`

This will run:
1) craftable new --no-install
2) Then it will add some env variables to both .env or .env.example
3) It will also manipulate some env variables
4) `habror composer require predis/predis`
5) `habror composer install`
6) `harbor restart`
7) `harbor artisan craftable:install`
8) `harbor npm install`
9) `harbor npm run dev`

D) I don't want to add my own code. In this case, you can skipt the initialization phase, and run just

`harbor start` 

# Starting a harbor (docker) #

To start the docker environment use:
`harbor start`

This will start all the docker containers and set up network and volumes correctly. So now we can play around with empty PostgreSQL or php. Now you can point your browser to the http://localhost and you should be able to see a default `/` route maped to public/index.php.

You can make any changes in .env file, but some changes (i.e. DB_PASSWORD, but you probably don't want to change these variables anyway) may require to destroy volumes and/or rebuild docker containers:

`harbor rebuild`

# Daily usage #

In this section you can find all commands supported by harbor.

`harbor start`

Starts all containers based on docker-compose.yml file.

`harbor stop`

Stops and destroy all containers.

`harbor restart`

Stops and destroy and then starts again all containers.

`harbor rebuild`

Stops and destroy all containers also with volumes, delete testing database path, rebuild docker images and then starts again all containers.

`harbor artisan` OR `harbor art`

Passes all additional arguments to php container to php artisan command, e.g. `harbor art make:migration` goes to php docker container and call `php artisan make:migration`.

`harbor composer` OR `harbor comp`

Passes all additional arguments to php container to composer command, e.g. `harbor comp require brackets/craftable` goes to php docker container and call `composer require brackets/craftable`.

`harbor test`

Run phpunit tests on new php container. All additional arguments are passed to phpunit.

`harbor npm`

Run npm command on node container and pass all additional arguments to npm.

`harbor yarn`

Run yarn command on node container and pass all additional arguments to yarn.

`harbor gulp`

Run gulp command on node container from node modules and pass all additional arguments to gulp.

`harbor psql`

Run psql command on pgsql container with username and host form .env file and pass all additional arguments to psql command.

`harbor pg_dump`

Run pg_dump command on pgsql container with username and host form .env file and pass all additional arguments to pg_dump command.

`harbor laravel new`

Will install laravel application to current folder. See section #Let's add some code

`harbor laravel init`

Will init harbor for laravel application. See section #Let's add some code

`harbor craftable new`

Will install craftable application to current folder. See section #Let's add some code.

`harbor craftable init`

Will init harbor for craftable application. See section #Let's add some code.













