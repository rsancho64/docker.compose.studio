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

- [x] created folder: `mkdir -p /home/ray/server/mysql-phpmyadmin/data/`

## phpMyAdmin container config

In the same way:

- `image` is the version of the phpMyAdmin image.
- `container_name` is the name of the container.
- `environment` are the environment variables (link to the MySQL db).
- `ports` host side first; container side second.
- `network` the one to which the container will be attached.

## Launch the containers

just type `docker compose up -d` in the folder w `docker-compose.yml` (must be this name) file:

`-d`: as 'daemon' (launch -the container- in the background).

If everything goes well networks n volumes should be created; then containers started

``` bash
[+] Running 30/2
 ✔ mysql 10 layers [⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿]      0B/0B      Pulled                              13.0s 
 ✔ phpmyadmin 18 layers [⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿]      0B/0B      Pulled                  8.1s 
[+] Running 2/2
 ✔ Container mysql       Started                                                     3.3s 
 ✔ Container phpmyadmin  Started                                                     0.5s 
 ```

Stops the containers:

``` bash
docker compose down
```

## phpMyAdmin

browse \`<http://localhost:8081/>\`\` the phpMyAdmin home page and login form.

Using identifiers specified in the `docker-compose.yml` file you have two options:

- if root: be admin of the MySQL SGBB.
    *Username* / **password**: *root* / **root_password**
- if user: be admin of the `database_name` db only.
    *Username* / **password**: *user_name* / **user_password**

## MySQL in command line

MySQL seems have not sh or bash. 

From the host machine, connect to the MySQL database from CLI:

``` bash
# sudo apt install mysql-client-core-8.0 ; 
mysql --host=127.0.0.1 --port=6033 -u root -p # as root (pass `root_password`)
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
mysql --host=127.0.0.1 --port=6033 -u user_name -p  # ray
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

## See also

- [How to install and run Portainer on Ubuntu 22.04?](/en/docker/how-to-install-portainer-on-ubuntu-22-04)
- [How to reset Portainer password?](/en/docker/how-to-reset-portainer-password)

- [ ] (https://josejuansanchez.org/bd/practica-07/index.html)


