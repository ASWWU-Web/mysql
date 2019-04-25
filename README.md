# MySQL
This repository contains the config files and infomration about the MySQL docker container.

## Deployment
Deployment is done through Docker Compose.
1. Copy the ``.env.sample`` file to ``.env`` and add the connection details. The parameters are as follows:

- ``MYSQL_ROOT_PASSWORD`` - The root password for the server. This should be **highly secure**.

2. To start the container, run the following command in the project root:

```
$ docker-compose up -d
```

The container can be rebuilt and restarted in place with:

```
$ docker-compose up -d --no-deps --build mysql
```

3. Thats it! Wow wasn't that easy.

## Connection and Configuration
By default, the new container will not have any databases. It will have a user ``root`` with the password defined in the ``.env`` file.

You can connect to the server mysql command line with the following command:

```
docker-compose exec mysql mysql -p
```

You will then be prompted for the root password. You can login as a different database user if you specify the ``-u`` option follwed by a username.

Yo should also adjust the ``[mysqld]`` section in the ``my.cnf`` file in ``/etc/mysql/`` (add it if the section does not exist) to have only the following parameters:

```
[mysqld]
default-storage-engine=INNODB
character_set_server=utf8mb4
innodb_default_row_format=DYNAMIC
innodb_large_prefix=ON
innodb_file_format=Barracuda
innodb_log_file_size=2G
```

This configuration step is required for the Jira server instance to work properly.

### Add database
After connecting to the MySQL shell, you can create new databases with the following command:

```
mysql> CREATE DATABASE database CHARACTER SET charset COLLATE collation;
```

If you dont know what charset and collation to use, good defaults are ``utf8mb4`` and ``utf8mb4_unicode_ci`` respectively.

### User management
It is ideal to create new users for each application that needs to access the server. The user's permissions should be currated to only allow access to necessary databases. You can add new users with the following command:

```
mysql> CREATE USER 'username'@'%' IDENTIFIED BY 'password';
```

Users can be deleted with the following command:

```
mysql> DROP USER username;
```

By default this new user will not have permission to access any databases, give them permission with the following command:

```
mysql> GRANT ALL PRIVILEGES ON database.* TO 'username'@'%';
```

This will grant them full access on the specfied database.

## Attaching networks
In order to attach Docker networks to the MySQL container, you must define a network in the service's Docker Compose network section:

```
networks:
  mysql:
    external:
      name: mysql
```

You can then specify the services networks:

```
services:
  theservices:
    networks:
      - mysql
```

The Jira repository provides a good example in its ``docker-compose.yml``.

## Backups
