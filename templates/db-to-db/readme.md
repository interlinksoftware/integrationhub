# db-to-db Template

The db-to-db template provides functionality to transfer, filter/transform and update data between databases using JDBC, via an integration-hub pipeline.

<p align="center">
<img src="../../assets/images/flow_db-to-db.jpg" />
</p>

***

## Install

Download the version of the db-to-db template that you require from github to your integration-hub server.

Install directly from Github
<font size="2">
```bash
ih-cli template import \
  https://raw.githubusercontent.com/interlinksoftware/integrationhub/main/templates/db-to-db/<version>/db-to-db~<version>.yml
```
</font>

If your server does not have access to Github you can download the file, copy to the server and place it directly into the ```integration-hub/config/templates``` directory or import via the integration-hub cli:
<font size="2">
```bash
ih-cli template import /path/to/file/db-to-db~<version>.yml
```
***
</font>

***

## Configure

This template will require you to install the relevant JDBC drivers and define the Data Sources that you wish to use.

### Installing JDBC Drivers

For the integration-hub to support the database(s) you wish to use, you will need to copy the relevant JDBC driver(s) jar files into the **integration-hub/config/lib** directory, then restart the integration-hub to make them available within the pipeline.

|database drivers|
---------|
|[Postgres][postgres_download]|
|[Oracle][oracle_download]|
|[Microsoft SQL Server][sqlserver_download]

[postgres_download]: https://jdbc.postgresql.org/download.html
[oracle_download]: https://www.oracle.com/uk/database/technologies/appdev/jdbc-downloads.html
[sqlserver_download]: https://docs.microsoft.com/en-us/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server?view=sql-server-ver15

***

### Defining Data Source Definitions

Configure the connection details for any new databases required for use in the pipeline.  The connection details are defined in the **integration-hub/config/datasources.yml** file on the integration-hub instance.

The following properties are mandatory for each connection. (For a comprehensive list of additional properties see the <a href="https://commons.apache.org/proper/commons-dbcp/configuration.html" target="_isspop">commons-dcp</a> specification):


|Property | Value|
|----------|------|
|driverClassName | Fully qualified Java class name of the JDBC driver to be used|
|url | JDBC connection Url|
|username | Database username|
|password | Encrypted database password|

You can refer to secrets from the management-service vault within the configuration file by enclosing them with `${}`:

```yml
  ${sqlserver_password}
```

For more information on how this is done refer to the vault section of the management-service on the Interlink documentation site.

#### Data Source Examples

```
    postgresExample:
        type: org.apache.commons.dbcp2.BasicDataSource
        driverClassName: org.postgresql.Driver
        username: ${postgres_username}
        password: ${postgres_password}
        url: "jdbc:postgresql://postgres.interlink.com:5432/mydb"

    sqlServerExample:
        type: org.apache.commons.dbcp2.BasicDataSource
        driverClassName: net.sourceforge.jtds.jdbc.Driver
        username: ${sqlserver_username}
        password: ${sqlserver_password}
        url: "jdbc:jtds:sqlserver://sqlserver.interlink.com;databaseName=mydb;"
        validationQuery: "select 1"
```
***

### Creating a pipeline

You will see the template listed within web-based application after you have installed the templates. Define a pipeline as you would through the web interface.
