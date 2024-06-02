# mongodb

This repository contains the source code and docker compose files for starting both a standalone and a replica set mongodb development environment based on docker.

## Folder mapping

- The `database` folder is mapped to the `/data/db` folder in the mongodb container. This means that any file you put in the `database` folder will be available in the mongodb container.

## How to use

1. Clone the repository
2. Run `docker compose up -d` to start the containers (older versions of docker cli should use `docker-compose up -d`)

## Connect with mongodb

First execute docker exec on the container.

    ```bash
    docker exec -it mongodb_container /bin/bash
    ```

Then use the mongo command to connect to the database.

    ```bash
    mongo -u root -p MyPassword_2024 --authenticationDatabase admin
    ```

## Create the user

```json
use admin
db.createUser ({ user: "myadmin", pwd: "1234567890", roles: [ { role: "userAdminAnyDatabase", db: "admin" }, "readWriteAnyDatabase", "clusterManager" ]})
db.grantRolesToUser("myadmin", ["clusterAdmin"])

```

Show users to check if new user exists

```json
show users
```

## Reauthenticate with the newly crated user

```bash
mongo -u myadmin -p 1234567890 --authenticationDatabase admin
```

## Import data

To import data into the database, you can use the `mongoimport` command.

Samples are donwloaded from https://github.com/neelabalan/mongodb-sample-dataset and copied on data mapped directory.

```bash
 mongoimport -u myadmin -p 1234567890 --authenticationDatabase admin --db ecommerce --collection listings_and_reviews --file /data/db/listingsAndReviews.json
```

Check if data has been imported

```json
use ecommerce
db.listings_and_reviews.count()
db.listings_and_reviews.find()
```

## Stop the server

Just use `docker compose down` to stop the server.

## Relaunch the server as replicaset

Uncomment the line with "command" in the `docker-compose.yml` file.

Connect with the container and launch mongo CLI.

Check if the replicaset is initialized.

```js
rs.status();
```

Response:

```json
{
  "info": "run rs.initiate(...) if not yet done for the set",
  "ok": 0,
  "errmsg": "no replset config has been received",
  "code": 94,
  "codeName": "NotYetInitialized"
}
```

Initialize replicaset

```js
rs.initiate();
```

Check if replicaset is initialized, again.

Check if data is still on the server.

```js
show dbs
use ecommerce
show collections
db.listings_and_reviews.find()
```

Check if oplog is active.

```js
use local
db.oplog.rs.find()
```

Sample response:

```json
{ "ts" : Timestamp(1717073040, 1), "h" : NumberLong("12081398464160618"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "initiating set" } }
{ "ts" : Timestamp(1717073042, 1), "t" : NumberLong(1), "h" : NumberLong("5303524809437918524"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "new primary" } }
{ "ts" : Timestamp(1717073052, 1), "t" : NumberLong(1), "h" : NumberLong("5991638086925658597"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "periodic noop" } }
{ "ts" : Timestamp(1717073062, 1), "t" : NumberLong(1), "h" : NumberLong("-2898813813534377312"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "periodic noop" } }
{ "ts" : Timestamp(1717073072, 1), "t" : NumberLong(1), "h" : NumberLong("-7957780126935780748"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "periodic noop" } }
{ "ts" : Timestamp(1717073082, 1), "t" : NumberLong(1), "h" : NumberLong("48898854351445808"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "periodic noop" } }
{ "ts" : Timestamp(1717073092, 1), "t" : NumberLong(1), "h" : NumberLong("-6323044827485632364"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "periodic noop" } }
{ "ts" : Timestamp(1717073102, 1), "t" : NumberLong(1), "h" : NumberLong("-7219213915177333660"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "periodic noop" } }
{ "ts" : Timestamp(1717073112, 1), "t" : NumberLong(1), "h" : NumberLong("-2529445123175565905"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "periodic noop" } }
{ "ts" : Timestamp(1717073122, 1), "t" : NumberLong(1), "h" : NumberLong("-570449250087073945"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "periodic noop" } }
{ "ts" : Timestamp(1717073132, 1), "t" : NumberLong(1), "h" : NumberLong("2225275533570990576"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "periodic noop" } }
{ "ts" : Timestamp(1717073142, 1), "t" : NumberLong(1), "h" : NumberLong("-7259514986469769563"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "periodic noop" } }
{ "ts" : Timestamp(1717073152, 1), "t" : NumberLong(1), "h" : NumberLong("363467638162061524"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "periodic noop" } }
{ "ts" : Timestamp(1717073162, 1), "t" : NumberLong(1), "h" : NumberLong("-5764259444305686240"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "periodic noop" } }
{ "ts" : Timestamp(1717073172, 1), "t" : NumberLong(1), "h" : NumberLong("4319834061009208514"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "periodic noop" } }
{ "ts" : Timestamp(1717073182, 1), "t" : NumberLong(1), "h" : NumberLong("6794270578621510383"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "periodic noop" } }
{ "ts" : Timestamp(1717073192, 1), "t" : NumberLong(1), "h" : NumberLong("6403886870966190755"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "periodic noop" } }
{ "ts" : Timestamp(1717073202, 1), "t" : NumberLong(1), "h" : NumberLong("231478679794836001"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "periodic noop" } }
{ "ts" : Timestamp(1717073212, 1), "t" : NumberLong(1), "h" : NumberLong("-6689946521085384567"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "periodic noop" } }
{ "ts" : Timestamp(1717073222, 1), "t" : NumberLong(1), "h" : NumberLong("-3685311393410811648"), "v" : 2, "op" : "n", "ns" : "", "o" : { "msg" : "periodic noop" } }
```

## Shutdown the replicaset (not needed)

```js
use admin
db.shutdownServer({force: true})
```
