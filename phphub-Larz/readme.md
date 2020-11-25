# Docker Nginx/PHP/MariaDB setup

This is configured using images available on DockerHub.

Everything is still customisable.

In this setup you get:
- NGINX 1.9.1
- PHP-fpm 7.4.9
- MariaDB 10.5.4 (2 instances)
- MariaDB 5.5.46
- phpmyadmin 5.0.2

I used these guides to set up PHP and NGINX:
- http://geekyplatypus.com/dockerise-your-php-application-with-nginx-and-php7-fpm/
- http://geekyplatypus.com/making-your-dockerised-php-application-even-better/

## Caution

This is not made to be production ready. 

## Starting/stopping the project

Clone this repo.

To start up:

1. open terminal
2. in terminal, navigate to the directory where you cloned this repo
3. run the following command
```
docker-compose up
```

> Be patient first time running above command. Docker will download the needed images, and build one image. These actions may take a while.

From your favorite browser you can access:
- NGINX server on localhost:8080
- PhpMyAdmin on localhost:7000

You can change the exposed ports in the `docker-compose.yml`-file.

Place your HTML/PHP-code in the `/src/site` directory.

### Stopping 
Simply press `ctrl-c` in the terminal where docker-compose is running

## Configuring the components

This is in no way a description of all ways to configure this project.
This is just the mere basics, but I have provided some links for you to explore. 

On Docker Hub there are more information for each component, as well as other versions to explore.

### Docker-compose

The `docker-compose.yml` is responsible for orchestrating everything.
- https://docs.docker.com/compose/

The `.env` file contains some parameters used in `docker-compose.yml`.

- https://docs.docker.com/compose/environment-variables/
- https://docs.docker.com/compose/env-file/

### NIGNX

NGINX specific configuration is located in `/config/nginx.site.conf`.

When running the image, the file mentioned above maps to `/etc/nginx/conf.d/default.conf` inside the docker container.

- [NGINX on Docker Hub](https://hub.docker.com/_/nginx)

### PHP
This image is a bit special - The PHP image is created using a dockerfile, specifically: `Dockerfile-php7-fpm-mysql`. This is done to activate MySQL support, needed to connect to MariaDB.

The `/config/php.log.conf` contains some configuration for logging, and when running the image, it maps to `/usr/local/etc/php-fpm.d/zz-log.conf` inside the docker container.

This setup uses the version of `php.ini` which is included in the build image. You can use your own by placing a file named `php.ini` in the `/config` folder, and uncommenting (remove the hash-tag/#) this line in `docker-compose.yml`:
```
#- ./config/php.ini:/usr/local/etc/php/php.ini
```

- [PHP on Docker Hub](https://hub.docker.com/_/php)

### MariaDB

MariaDB is configured from `docker-compose.yml` and `.env`.

The same password is used on all 3 instances.

All databases are configured to save their data in subfolders in the `/mariadb` folder. This way you will have your data the next time you run `docker-compose up`.

Simply delete the subfolders in `/mariadb`, to get a fresh start. (but leave the `/mariadb` folder, or else you will get an error)

- [MariaDB on Docker Hub](https://hub.docker.com/_/mariadb)

### phpMyAdmin

phpMyAdmin is configured to show all 3 databases. You just need to select one, and log in.

- [phpMyAdmin on Docker Hub](https://hub.docker.com/r/phpmyadmin/phpmyadmin)

# Other tools
I use Portainer to get a better overview of my Docker images, running containers and other stuff Docker does.
- https://www.portainer.io/