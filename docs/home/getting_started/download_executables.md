# Download Executables

## Requirements

* Make sure you have the proper version of **Java** installed.
* Make sure you have the proper version of **MySQL** installed.

See details [here](../implementation.md).

>**Hint:** [MySQL connections configuration](../../help/faq.md#mysql-connections)

## Download and prepare

* Go to [releases](../../downloads/releases.md) and download the`.zip` files for the choosen systems.
* Unzip the files.
* Go to the `database` folder and execute `mysql -u {username} -p < create_empty_db.sql` command in order to create the database.

## Start the core systems

* Make sure you have the proper values in the configuration file before starting the required core systems.
    
    > The system configuration properties can be found in the `application.properties` file which is located next to the executable `.jar` file of the system. Look for the configuration possibilities in the system descriptions ([example](../../core_systems/service_registry.md#configuration)). 

* Execute `java -jar arrowhead-{core-system}-{version}.jar` from the same folder.
* Note that always the ServiceRegistry Core System has to be started first. The other ones should be started only when ServiceRegistry is up and running.