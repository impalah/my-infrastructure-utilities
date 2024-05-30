# php-mysql

This repository contains the source code and docker compose files for starting a php+mysql development environment based on docker.

## Folder mapping

- The `www` folder is mapped to the `/var/www/html` folder in the php container. This means that any file you put in the `www` folder will be available in the php container.
- The `database` folder is mapped to the `/var/lib/mysql` folder in the mysql container. This means that any file you put in the `database` folder will be available in the mysql container.
- The nginx configuration file is located in the `nginx-conf` folder and mapped to the `/etc/nginx/conf.d/default.conf` folder in the nginx container. This means that any file you put in the `nginx-conf` folder will be available in the nginx container.

## How to use

1. Clone the repository
2. Run `docker compose up -d` to start the containers (older versions of docker cli should use `docker-compose up -d`)
3. Open your browser and go to `http://localhost/info.php` to see the php info page.
4. Open your browser and go to `http://localhost:8083` to see the phpmyadmin page.

## Stop the server

Just use `docker compose down` to stop the server.

## Custom php modules

If you need to add custom php modules, you can do so by adding the module to the `Dockerfile` and rebuilding the image.

An example is provided that includes the `mysqli` `pdo` and `pdo_mysql` modules.

To launch the custom php use the command `docker compose -f docker-compose.custom.yml up -d --build`

To stop the custom php use the command `docker compose -f docker-compose.custom.yml down`
