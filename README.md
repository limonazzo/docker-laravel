# Docker, docker-compose for Laravel development

This repository provides you a development environment for Laravel; it requires Docker and Docker Compose installed in your system.
This repository has a web server ( Apache on Debian ), database server (MySQL) , and PhpMyAdmin.


## Installation

1. Install [docker](https://docs.docker.com/engine/installation/) and [docker-compose](https://docs.docker.com/compose/install/) ;

2. Copy `docker-compose.yml` file and `php7.4` folder to your project root path, and edit it according to your needs ;

3. From your project directory, start up your application by running:

```sh
docker-compose up
```

4. If you work on *nix system, it might appear a write permissions error. To solve this you have to re-map [¹](#explanation) the user of the host machine with the user `www-data` inside the virtual machine, and restart the services ( `Ctrl+c`   and    `docker-compose up` again )

```sh
docker-compose exec webserver usermod -u $(id -u) www-data
docker-compose exec webserver groupmod -g $(id -g) www-data
```

5. You can run composer or artisan through docker. In another teminal you must run: [¹](#explanation)

```sh
docker-compose exec --user www-data webserver composer
docker-compose exec --user www-data webserver php artisan
```
On windows you must run withou the user flag:

```sh
docker-compose exec webserver composer
docker-compose exec webserver php artisan
```

## Configure .env

The database must be configured according to database service name in `services` definition:

```sh
services:
  database: # <- this name 
    image: mysql:5.7.31
```

```sh
DB_CONNECTION=mysql
DB_HOST=database # <- here ; in this case is just 'database'
DB_PORT=3306
DB_DATABASE=laravel_db
DB_USERNAME=laravel_user
DB_PASSWORD=laravel_password
```

## Licence

This work is under [MIT](LICENCE) licence.


<a name="explanation"></a>

## re-map Users explanation *

The kernel uses a number to distinguish users and groups; when a file is created (regardless of whether it is inside or outside the virtual machine) the numbers are assigned to the file. In this way a file that has user 1000 in the host machine can be 'limonazzo' user and inside the virtual machine it can be 'www-data' user at the same time; in this way we avoid the horrible `chmod 777 *` command.

✌️🍋 Saludos.