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

## Attaching networks

## Backups
