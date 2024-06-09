# mysql

replicas!!!

## Folder mapping

- The `database` folder is mapped to the `/var/lib/mysql` folder in the mysql container. This means that any file you put in the `database` folder will be available in the mysql container.

## How to use

1. Clone the repository
2. Run `docker compose up -d` to start the containers (older versions of docker cli should use `docker-compose up -d`)

## Stop the server

Just use `docker compose down` to stop the server.

## Start replication

1. Connect with master node

```bash
docker exec -it master_container mysql -u root -pIamGr00t
```

2. Create a user for replication

On master server

```sql
CREATE USER 'replica'@'%' IDENTIFIED BY 'IamB4byGr00t';
GRANT REPLICATION SLAVE ON *.* TO 'replica'@'%';
FLUSH PRIVILEGES;

SHOW MASTER STATUS;

+------------------+----------+--------------+------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+------------------+----------+--------------+------------------+
| mysql-bin.000005 |      417 |              |                  |
+------------------+----------+--------------+------------------+


```

3. Connect with replica node

```bash
docker exec -it replica_container mysql -u root -pIamGr00t
```

4. Configure the replica

```sql
CHANGE MASTER TO
  MASTER_HOST='master_container',
  MASTER_USER='replica',
  MASTER_PASSWORD='IamB4byGr00t',
  MASTER_LOG_FILE='mysql-bin.000004',
  MASTER_LOG_POS=417;

-- Inicia la replicación
START SLAVE;
SHOW SLAVE STATUS;

```

5. Test the replication

On replica server

```sql
SHOW VARIABLES LIKE 'log_bin';
SHOW VARIABLES LIKE 'binlog_format';
SHOW VARIABLES LIKE 'binlog_do_db';
SHOW VARIABLES LIKE 'binlog_ignore_db';
```

### Create replication user and assign permissions

```sql
CREATE USER 'replicator'@'%' IDENTIFIED BY 'IamB4byGr00t';
GRANT SELECT, SUPER, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'replicator'@'%';
FLUSH PRIVILEGES;
```
