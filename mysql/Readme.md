# mysql

This repository contains the source code and docker compose files for starting several configurations of mysql on docker.

## Folder mapping

- The `database` folder is mapped to the `/var/lib/mysql` folder in the mysql container. This means that any file you put in the `database` folder will be available in the mysql container.

## How to use

1. Clone the repository
2. Run `docker compose up -d` to start the containers (older versions of docker cli should use `docker-compose up -d`)

## Stop the server

Just use `docker compose down` to stop the server.

