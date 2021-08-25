# MySQL 5.7

https://dev.mysql.com/doc/refman/5.7/en/docker-mysql-more-topics.html

```bash
docker run \
--name=mysql5.7 \
--env="MYSQL_ROOT_HOST=%" \
--env="MYSQL_ROOT_USERNAME=root" \
--env="MYSQL_ROOT_PASSWORD=master" \
--mount type=bind,src=/Users/eugenmaysyuk/docker/mysql/etc/my.cnf,dst=/etc/my.cnf \
--mount type=bind,src=/Users/eugenmaysyuk/docker/mysql/var/lib/mysql,dst=/var/lib/mysql \
--detach \
-p 3306:3306 \
mysql/mysql-server:5.7.19
```

```bash
docker stop mysql5.7
```

```bash
docker start mysql5.7
```

```bash
docker exec -it mysql5.7 bash
```

```sql
select User, Host from mysql.user;
update mysql.user set host = '%' where user='root';
update mysql.user set host = '%' where user='healthchecker';
```



**DO NOT MOUNT /var/lib/mysq/data directory to docker container** as MySQL uses files system of this directory to identify value for lower_case_table_names setting.
However, this is not the issue anymore for MySQL 8.0 server as that version of server stores all tables in files with lowercase names.

> If /var/lib/mysql mounted to container, file system will be case insensitive (macOS, Windows) and MySQL will set lower_case_table_names=2 which will cause issue because MySQL is actually running on Linux (case sensitive system)**

> Correct values for lower_case_table_names should be:
> lower_case_table_names=2 (macOS and Windows)
> lower_case_table_names=0 (Unix/Linux)

> The mysql server calculates value for lower_case_table_names during initial setup based on File System (/var/lib/mysql/data)
See here for more details: https://dev.mysql.com/doc/refman/8.0/en/identifier-case-sensitivity.html

**If you need to mount /var/lib/mysql folder for mysql/mysql-server:5.7.19 you need to explicitly set lower_case_table_names=0**

# MySQL 8.0

```bash
docker run --name mysql8.0 -v /Users/eugenmaysyuk/docker/mysql/etc/my.cnf:/etc/my.cnf -p 3306:3306 -d -e MYSQL_ROOT_PASSWORD=master mysql:8.0
```

