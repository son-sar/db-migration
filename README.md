## Database Migration from MariaDB to MySQL

## Description
Migrate from MariaDB to MySQL starting from local instances. 
Deploy both databases using Docker following the offical documentation.

## How to install and run the Project
1. Install Docker on your local machine
https://docs.docker.com/desktop/install/mac-install/

2. Deploy both databases using Docker following the official documentation starting with MariaDB
https://hub.docker.com/_/mariadb < mariadb 
2.1. Install MariaDB image on Docker
Via Docker Compose
I started a mariadb server instance via docker-compose.yaml (link to file) --> the container is running / Visit http://localhost:8080 and you will see the adminer UI (screenshot)
OR Shell
using the following commands: 
$ docker network create some-network 
$ docker run --detach --network some-network --name some-mariadb --env MARIADB_USER=example-user --env MARIADB_PASSWORD=my_cool_secret --env MARIADB_ROOT_PASSWORD=my-secret-pw  mariadb:latest
$ docker run -it --network some-network --rm mariadb mysql -hsome-mariadb -uexample-user -p
--> container (some-mariadb) is running

2.2. Create a MariaDB sample database as root in the shell: 
$ docker run -it --network some-network --rm mariadb mysql -hsome-mariadb -uroot -p 
Inside the standard MariaDB prompt, you can add a sample database with: 
MariaDB [(none)]> CREATE DATABASE sunny

3. Export the database as a dump (aka Creating database dumps): 
$ docker exec some-mariadb sh -c 'exec mysqldump --all-databases -uroot -p"$MARIADB_ROOT_PASSWORD"' > /Users/so/Tasks/db-migration/my-dump/all-databases.sql 
The dump is saved locally as all-databases.sql

// more on dumps: https://www.digitalocean.com/community/tutorials/how-to-import-and-export-databases-in-mysql-or-mariadb

4. Install MySQL image on Docker
https://hub.docker.com/_/mysql < mysql
$ docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:latest
--> container running

5. Import the database from the dump
// Article on the matter: https://lefred.be/content/migrate-from-mariadb-to-the-mysql-on-centos/

Do I need 5.5.60-MariaDB version to import from MySQL? (How to change the version on MariaDB: https://mariadb.com/kb/en/downgrading-between-major-versions-of-mariadb/)

Restoring data from dump files:
$ docker exec -i some-mysql sh -c 'exec mysql -uroot -p"$MYSQL_ROOT_PASSWORD"' < /Users/so/Tasks/db-migration/my-dump/all-databases.sql

$ docker exec -i some-mysql sh -c 'exec mysql -uroot -p"my-secret-pw"' < /Users/so/Tasks/db-migration/my-dump/all-databases.sql


ERROR:
mysql: [Warning] Using a password on the command line interface can be insecure.
ERROR 1064 (42000) at line 72: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'PAGE_CHECKSUM=1 TRANSACTIONAL=0 COMMENT='Statistics on Columns'' at line 14

Tried to fix it with the followong approach: https://stackoverflow.com/questions/30759313/mysql-database-import-error-1064
Didn't help

// Do I need a certain MariaDB version for the import? What else?

// What about this: https://www.mysql.com/products/workbench/migrate/ 

// Notes: 
- Secrets in Docker
- Where to store data

Extras:

Write a docker-compose.yaml to specify the steps

Migrating from MariaDB to MySQL (Isn’t it downgrading?) 
https://lefred.be/content/migrate-from-mariadb-to-the-mysql-on-centos/

## How to use the Project

## Credits

## License

