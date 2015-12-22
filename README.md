Phundament 4
============

Phundament is a dockerized 12factor PHP application template for Yii Framework 2.0.

Full [documentation](https://github.com/phundament/docs).
See Phundament playground  for a [demo](https://github.com/phundament/playground).


Requirements
------------

- [docker](https://docs.docker.com/engine/installation/)
- [docker-compose](https://docs.docker.com/compose/) **>=1.5.2**

> For alternative installation methods, such as composer, see the [docs](https://github.com/phundament/docs).  


Installation
------------

[Download](https://github.com/phundament/app/releases) or clone the repository and go to the application directory

    git clone https://github.com/phundament/app
    cd app

### With `make`

    make all
     
> On OS X, the new application should open automatically in your default browser.

### Without `make`

Create environment configuration file    
    
    cp .env-dist .env
    cp docker-compose.override-dist.yml docker-compose.override.yml

Start the application stack

    docker-compose up -d

Run setup commands
    
    docker-compose run php composer install
    docker-compose run php sh src/setup.sh

### Additional information

After startup is complete, open `http://docker:40080` to access the application and login with `admin`/`admin`.

List all services    
    
    docker-compose ps

Show and follow logs    
    
    docker-compose logs


Configuration
-------------

### Environment overrides - `docker-compose.override.yml`

- host-volumes for local development
- port mappings

### Environment defaults - `docker-compose.yml`

> You can override any ENV variable in `.env` within a `docker-compose.yml` file.
     
 - `VIRTUAL_HOST` `~^myapp\.` Virtual-host configuration for reverse proxy, adjust the virtual host parameter 
    for web application, we'll use it later to easily access the web-server through a wildcard DNS.

### Application defaults - `.env`

> During development, it is recommended to change application configuration in the `.env` file, since it does not require restarting the containers. 

*Identifier*

 - `APP_NAME` unique application and container identifier *[a-z0-9]*
 - `APP_TITLE` display name of the application

*Application*
 
 - `APP_MIGRATION_LOOKUP` comma separated list of Yii aliases to look for database migrations, eg. `@app/migrations/data`
 - `APP_ADMIN_EMAIL` e-mail address of application admin user (default in `./yii app/create-admin-user`)
 - `APP_ADMIN_PASSWORD` password of application admin user (default in `./yii app/create-admin-user`)
 - `APP_SUPPORT_EMAIL` e-mail address for the application, eg. `support@myapp.local`
 - `APP_COOKIE_VALIDATION_KEY` unique and random string to prevent XSS
 - `APP_PRETTY_URLS` enable or disable nice URLs, allowed values `1` (yes) or `0` (no)

*Application development settings*

 - `APP_ASSET_FORCE_PUBLISH` force asset publishing after any changes to asset files. **Note!** This may degrade performance, use *only during development*.

*Framework*
 
 - `YII_DEBUG` wheter to enable more verbose application output, eg. on PHP exceptions.
 - `YII_ENV` Yii application mode, allowed values `dev`, `prod` or `test`
 - `YII_TRACE_LEVEL` amount of caller levels to display for logging.
 
*Database*
 
 - `DB_ENV_MYSQL_ROOT_USER` user to create databases
 - `DB_ENV_MYSQL_ROOT_PASSWORD` root password, eg. set from `"${DB_ENV_MARIADB_PASS}"`
 - `DB_ENV_MYSQL_DATABASE` database name
 - `DB_ENV_MYSQL_PASSWORD` database password
 - `DB_ENV_MYSQL_USER` database user
 - `DB_PORT_3306_TCP_ADDR` database hostname
 - `DB_PORT_3306_TCP_PORT` database port
 - `DATABASE_TABLE_PREFIX` table prefix for default database connection


### PHP Application settings - `config/main.php`

For details of available application configuration, please refer to the Yii 2.0 Framework Definitive Guide. 


Testing
-------

### Without `make`

First, build your application image

    docker-compose build 

Set environment variables for test stack

    export COMPOSE_PROJECT_NAME=testapp
    export TEST_IMAGE_PREFIX=app
    export HOST_APP_VOLUME=.

Start test stack and enter tester CLI container

    docker-compose -f docker-compose.yml -f build/compose/test.override.yml up -d    
    docker-compose -f docker-compose.yml -f build/compose/test.override.yml run tester bash    

Setup application *(container bash)*    
    
    $ sh src/setup.sh

Run test suites *(container bash)*

    $ codecept run functional prod
    $ codecept run acceptance prod


### With `make`

Or one-by-one via `Makefile` targets, make sure to build first, if you have made changes to `src`

    make build
    
Start the test stack    
    
    make TEST setup up 

Enter the `tester` container    
    
    make TEST bash

Run codeception directly *(container bash)*

    $ codecept run acceptance allow_fail

Or run the test suites from build scripts

    $ sh build/scripts/build.sh
    $ sh build/scripts/test.sh

> :information_source: `YII_ENV` must be set to `test` when running codeception.


Deployment
----------

Variables for pushing docker images.

- `REGISTRY_USER`
- `REGISTRY_PASS`
- `REGISTRY_HOST`
- `IMAGE_NAME`


Links
-----

- [Documentation](https://github.com/phundament/docs)
- [Project Source-Code](https://github.com/phundament/app)
- [Website](http://phundament.com)
- [Team](https://github.com/orgs/phundament/teams)
- [Imprint](http://herzogkommunikation.de/de/impressum-7.html)

-----------

Built by *dmstr, Stuttgart :de:

