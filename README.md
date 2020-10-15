# Docker Compose of Apache Airflow

A very simple docker-compose of Apache Airflow.

This repo is only used on my Raspberry Pi 4 with Ubuntu 20.04 (ARM64).

## Build the image on Raspberry Pi 4

Some of the dependencies requires to compile based on the architecture of hardware and OS, so the docker image need to be built on Raspberry Pi 4.

I pushed the images to **Github Container Registry** already:

* ghcr.io/grammy-jiang/airflow:latest (ghcr.io/grammy-jiang/airflow:2.0.0.dev0)
* ghcr.io/grammy-jiang/airflow:2.0.0.dev0

The command to push the image to **Github Container Registry**:

```console
foo@bar:~$ docker push ghcr.io/grammy-jiang/airflow:<tag>
...
```

The command to pull the image from **Github Container Registry**:

```console
foo@bar:~$ docker pull ghcr.io/grammy-jiang/airflow:<tag>
...
```

## PostgreSQL as backend

In this repo, PostgreSQL is chosen as backend. In the document of Apache Airflow, the version 10 of PostgreSQL is [supported](https://github.com/apache/airflow#requirements) by the latest version of Apache Airflow.

## Prepare

### Initialize PostgreSQL database and user

Create a docker container to initialize PostgreSQL database and user which mentions in [`postgres.env`](https://github.com/grammy-jiang/dc-airflow/blob/master/postgres.env).

### Initialize Apache Airflow

## Prepare `$AIRFLOW_HOME`

Create the folder of `$AIRFLOW_HOME`, and assign the right owner:

```console
foo@bar:~$ mkdir airflow
foo@bar:~$ sudo chown 50000:0 airflow
...
```

Initialize Apache Airflow:

```console
foo@bar:~$ docker run --rm -v "/home/grammy-jiang/projects/dc-airflow/airflow:/opt/airflow" ghcr.io/grammy-jiang/airflow:latest db init
```

In the container, `$AIRFLOW_HOME` is `/opt/airflow` ([airflow/Dockerfile at master · apache/airflow](https://github.com/apache/airflow/blob/master/Dockerfile#L41)).

The content of `airflow`:

```console
foo@bar:~$ k -h airflow
total 608
-rw-r--r-- 1 50000 50000  35K 13 Oct   15:27 + airflow.cfg 
-rw-r--r-- 1 50000 50000 248K 13 Oct   15:27 + airflow.db 
drwxr-xr-x 3 50000 50000 4.0K 13 Oct   15:27 | logs 
-rw-r--r-- 1 50000 50000 2.3K 13 Oct   15:27 + unittests.cfg 
-rw-r--r-- 1 50000 50000 4.4K 13 Oct   15:27 + webserver_config.py 
```

### Create the first user of Apache Airflow

```console
foo@bar:~$ docker run \
--env-file airflow.env \
--link dc-airflow-postgres \
--network dc-airflow_nw-airflow \
--rm \
--volume "/home/grammy-jiang/projects/dc-airflow/airflow:/opt/airflow" \
ghcr.io/grammy-jiang/airflow:latest \
users create \
--email grammy-jiang@gmail.com \
--firstname Grammy \
--lastname Jiang \
--password <password> \
--role Admin \
--username grammy-jiang
[2020-10-13 05:12:53,481] {dagbag.py:431} INFO - Filling up the DagBag from /opt/airflow/dags
[2020-10-13 05:12:54,569] {manager.py:719} WARNING - No user yet created, use flask fab command to do it.
Admin user grammy-jiang created.
```

## Apache Airflow configruation

The configuration file is created in `./airflow/airflow.cfg`.

For this docker-compose, the configuration is in [`airflow.env`](https://github.com/grammy-jiang/dc-airflow/blob/master/airflow.env).

* expose the configuration: https://airflow.apache.org/docs/stable/configurations-ref.html#expose-config

# Reference

* [apache/airflow: Apache Airflow - A platform to programmatically author, schedule, and monitor workflows](https://github.com/apache/airflow)
* [apache/airflow - Docker Hub](https://hub.docker.com/r/apache/airflow)
* [Engine Configuration — SQLAlchemy 1.3 Documentation](https://docs.sqlalchemy.org/en/13/core/engines.html)
* [mysql/mysql-server - Docker Hub](https://hub.docker.com/r/mysql/mysql-server)
* [Compose file version 3 reference | Docker Documentation](https://docs.docker.com/compose/compose-file/)
