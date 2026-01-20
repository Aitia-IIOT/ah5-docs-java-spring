# Compile Code

## Requirements

* Make sure you are familiar with the [basics](../../help/definitions.md).
* Make sure you have the proper version of **Java** installed.
* Make sure you have the proper version of **Maven** installed.
* Make sure you have the proper version of **MySQL** installed.

See details [here](../implementation.md).

>**Hint:** [MySQL connections configuration](../../help/faq.md#mysql-connections)

## Compile the source code

* Download or clone the source code repositories of the choosen Core/Support systems and the common library [from GitHUB](../../contribute/code-contribution.md#github-repositories).
* Execute `mvn clean install -DskipTests` in the root folder of the common library first, and then in the root folder of each repository.
* After the build is complete, the executable `jar` file with the appropriate `application.properites` and `log4j2.xml` configuration files will be available in `<system-folder>/target` directory.
    - By default the test certificates will be built into the executable jar files. If you want your own certificates being built-in, then place them into the `<system-folder>/src/main/resources/certificate` folder before executing the above mentioned maven command.
* Execute `mysql -u <username> -p < create_empty_db.sql` command in the `<system-folder>/src/main/resources/database` folder for each system in order to create the database (if needed).

## Start the Core/Support systems

* Make sure you have the proper values in the configuration file before starting the required core systems.
    
    > The system configuration properties can be found in the `application.properties` file which is located next to the executable `.jar` file of the system. Look for the configuration possibilities in the system descriptions ([example](../../core_systems/service_registry.md#configuration)). 

* Execute `java -jar arrowhead-{core-system}-{version}.jar` from the same folder.
* Note that always the ServiceRegistry Core System has to be started first. The other ones should be started only when ServiceRegistry is up and running.