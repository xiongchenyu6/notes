---
title: "mysql"
date: 2022-10-20
---

``` sql

CREATE USER 'gitea'@'%' IDENTIFIED BY 'gitea';

GRANT ALL PRIVILEGES ON *.* TO 'gitea'@'%' WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON *.* TO 'user1'@localhost IDENTIFIED BY 'password1';
GRANT ALL PRIVILEGES ON 'yourDB'.* TO 'user1'@localhost;

-- mariadb
CREATE USER 'itoken'@'%' IDENTIFIED WITH mysql_native_password
SET PASSWORD FOR 'itoken'@'%' = PASSWORD('<REDACTED>');
GRANT ALL PRIVILEGES ON *.* TO 'itoken'@'%' WITH GRANT OPTION;

CREATE USER 'itoken'@'%' IDENTIFIED WITH mysql_native_password
SET PASSWORD FOR 'itoken'@'%' = PASSWORD('');
GRANT ALL PRIVILEGES ON *.* TO 'itoken'@'%' WITH GRANT OPTION;


CREATE USER 'itoken-master'@'%' identified by 'itoken-master';
GRANT REPLICATION SLAVE ON *.* TO 'itoken-master'@'%';
FLUSH PRIVILEGES;
SHOW MASTER STATUS;       
```

nix localhost don\'t need password
