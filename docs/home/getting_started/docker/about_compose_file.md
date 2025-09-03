# About Compose File

* `application.properties`, `log4j2.xml` and certificate folder will be copied to the host at first time.
* DB schema will be created on first startup when `/var/lib/mysql` in the docker container is empty.
* `SPRING_DATASOURCE_URL` environment variable overides the `spring.datasource.url` in the `application.properties`.
* In case of aggregated and coupled compose the system is accessing the database through an internal docker network.
* The database can be accessed by the human operators via the exposed port and with the given credentials (`MYSQL_USER`, `MYSQL_PASSWORD`) if necesarry.
* `DOMAIN_NAME` environment variable overides the `domain.name` in the `application.properties`. 
* `SERVICE_REGISTRY_ADDRESS` environment variable overides the `service.registry.address` in the `application.properties`.
* `SERVICE_REGISTRY_PORT` environment variable overides the `service.registry.port` in the `application.properties`.
* Do not use `localhost` or `127.0.0.1` to address the Arrowhead Core or Support systems! Use always the real IP address of the given host machine. Or alternatively you can use the `172.17.0.1` docker bridge gateway address, but only within the same host machine that runs the docker engine.
* To change any environment variable requires to recreate the related container.
    * Recreating a system container always overrides the bind mounts, so the `/config` folder content will be set back to the defaults. Always make sure to backup your configuration before system container recreation.
    * Recreating a database container does not effect the named volumes, so the new container will use the same persistent database files.  