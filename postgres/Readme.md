# PostgreSql

This repository contains the docker compose files for starting several kinds of postgresql database configurations

## Folder mapping

- The `database` folder is mapped to the `/var/lib/postgresql/data` folder in the postgresql containers. This means that any file you put in the `database` folder will be available in the postgresql containers.

## How to use

1. Clone the repository
2. Run `docker compose up -d` to start the containers (older versions of docker cli should use `docker-compose up -d`)

## Connect with postgresql

First execute docker exec on the container.

    ```bash
    docker exec -it postgres_container /bin/bash
    ```

Then use the mongo command to connect to the database.

    ```bash
    export PGPASSWORD='MyP4\$\$w0rd'
    psql -v ON_ERROR_STOP=1 --username "admin" --dbname "demo"
    ```

Use \l to see the databases

    ```bash
    \l
    ```

Use `\c` to connect to a database

    ```bash
    \c demo
    ```

Use `\dt` to see the tables in the current database

    ```bash
    \dt
    ```

Use \q to quit psql

    ```bash
    \q
    ```

Use \dx to view active extensions

    ```bash
    \dx
    ```

Show avalaible extensions

    ```sql
    SELECT * FROM pg_available_extensions 
    WHERE name NOT IN (SELECT extname FROM pg_extension);
    ```


