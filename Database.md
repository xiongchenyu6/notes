---
title: "Database"
date: 2021-11-20
---

# CAP

## ACID

- atomicity

each transaction is treated as a single "unit", which either succeeds completely, or fails completely.

- consistency

Consistency ensures that a transaction can only bring the database from one valid state to another

- isolation

Isolation ensures that concurrent execution of transactions leaves the database in the same state that would have been obtained if the transactions were executed sequentially.

- durability

Durability guarantees that once a transaction has been committed, it will remain committed even in the case of a system failure

## BASE

- Basically Avaliable
- Soft state

# MySQL

## Fresh set up on arch Linux

```bash
systemctl stop mysql
rm -R /var/lib/mysql/*
mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
systemctl start mysql
```

``` mysql
show database;
show tables;
use xxx;
```

## Export SQL

```bash
mysqldump db_name > backup-file.sql
```

## Import SQL

```bash
mysql db_name < backup-file.sql
```

# Neo4j

delete all node

``` neo4j
MATCH (n)
DETACH DELETE n
```
[[systemd]]