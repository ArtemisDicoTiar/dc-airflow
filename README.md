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

# Reference

* [apache/airflow: Apache Airflow - A platform to programmatically author, schedule, and monitor workflows](https://github.com/apache/airflow)
* [apache/airflow - Docker Hub](https://hub.docker.com/r/apache/airflow)
* [Engine Configuration — SQLAlchemy 1.3 Documentation](https://docs.sqlalchemy.org/en/13/core/engines.html)
* [mysql/mysql-server - Docker Hub](https://hub.docker.com/r/mysql/mysql-server)
* [Compose file version 3 reference | Docker Documentation](https://docs.docker.com/compose/compose-file/)
