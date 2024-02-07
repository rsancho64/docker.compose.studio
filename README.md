# Un estudio de docker compose utilizando dos servicios: mysql y phpmyadmin ...

... y almacenando los datos en el host local.

Lo hacemos con [este taller](https://lucidar.me/en/docker/mysql-and-phpmyadmin-with-docker-compose/)

## intro

we use **Docker** to run both **MySQL** and **phpMyAdmin** by creating a
**`docker-compose.yml`** file with sections:

- version
- network
- containers (services)

``` yaml
version: '3' # first line: specify the version :
```

## Network

A simple network: common to MySQL n phpMyAdmin: The `mysql-phpmyadmin` network uses the `bridge` to be reachable from the host machine:

## Volume

We want the db stored in the user\'s folder. Let\'s create a volume `mysqldata` and specify it in the host machine folder.

## MySQL container config

- image` is the version of the MySQL image.
- `container_name` is the name of the container.
- `environment` are the env vars (db identifiers).
- `ports` host side first; container side second.
- `volumes` is the volume where the data are stored.
- `network` the one to which the container will be attached.
    

## phpMyAdmin container config

In the same way:

- `image` is the version of the phpMyAdmin image.
- `container_name` is the name of the container.
- `environment` are the environment variables (link to the MySQL db).
- `ports` host side first; container side second.
- `network` the one to which the container will be attached.

## Launch the containers

just type `docker compose up -d` in the folder w `docker-compose.yml` file:

`-d`: as 'daemon' (launch -the container- in the background).

If everything goes well networks n volumes should be created; then containers started

``` bash
[+] Running 3/3
 ⠿ Network mysql-phpmyadmin  Created                                                              0.1s
 ⠿ Container mysql           Started                                                              0.6s
 ⠿ Container phpmyadmin      Started
```

Stops the containers:

``` bash
docker compose down
```

## phpMyAdmin

In the browser \`<http://localhost:8081/>\`\` the phpMyAdmin home page should display login form.

You have two options to connect to it:

- *Username* / **password**: *root* / **root_password**
- *Username* / **password**: *user_name* / **user_password**

These are obviously the identifiers specified in the `docker-compose.yml` file.

In the first case, you will be administrator of the MySQL DB.
In the second case, you will only be able to administrate the `database_name` db

## MySQL in command line

From the host machine, connect to the MySQL database from CLI:

``` bash
mysql --host=127.0.0.1 --port=6033 -u root -p # connect as root (pass `root_password`)
```

``` sql
mysql> SHOW DATABASES; -- as administrator manage all DBs:
+--------------------+
| Database           |
+--------------------+
| database_name      |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0,01 sec)
```

Or as a user:

``` bash
mysql --host=127.0.0.1 --port=6033 -u user_name -p
```

You will only have access to the databases to which your user has privileges:

``` sql
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| database_name      |
| information_schema |
| performance_schema |
+--------------------+
3 rows in set (0,00 sec)
```

## Download

You can download the complete file below. Note that you have to **rename
it `docker-compose.yml` to start the containers** :

[
mysql-phpmyadmin-docker-compose.yml](../files/mysql-phpmyadmin-docker-compose.yml)

## See also

- [How to install and run Portainer on Ubuntu 22.04?](/en/docker/how-to-install-portainer-on-ubuntu-22-04)
- [How to reset Portainer password?](/en/docker/how-to-reset-portainer-password)
