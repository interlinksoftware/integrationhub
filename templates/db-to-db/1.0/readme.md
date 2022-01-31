# db-to-db v1.0

## Pipeline Prerequisites

Before creating the pipeline you will need to gather the following information:

- The connection details for both data sources (source and destination), configured in the **datasources.yml** file.
- JDBC compatible drivers and connection properties for each data source installed within integration-hub.
- SQL query to be performed on the source to retrieve the required data.  This can be used with the checkpoint placeholder to avoid reading duplicates (see [checkpoint example](#using-a-checkpoint)).
- SQL update/insert to be performed on the destination data source.  This can be a procedure or function depending on the complexity of the update (see [upsert example](#upsert-stored-procedure-example)).
- Determine the frequency of executing the pipeline, taking into consideration the amount of data being processed and other pipelines executing within the integration-hub instance.

***

## Create the Pipeline

Create the pipeline file in the **integration-hub/config** directory.

:bulb: The pipeline file is in YAML format.

There are two examples below for both checkpoint and stored procedure method types.

### Using a checkpoint

Using a checkpoint ensures only new data that matches your _sourceSqlStatement_ is processed on each read.  Each time the _sourceSqlStatement_ is executed the checkpoint value from the previous execution is used. In the example below, the checkpoint key is the *alertid*, which ensures only rows where the *alertid* is greater than the previous execution are returned.

Example _besalerts_ pipeline: **config/besalerts.yml**:

```yml
app:
  pipelines:
    "[besalerts]":
      steps:
        - method: pipeline-template
          ref: "db-to-db~1.0"
          properties:
            schedule: "0 * * ? * *"
            triggerName: "besalerts-trigger"
            sourceDataSource: "devbes_db"
            targetDataSource: "uatbes_db"
            sql:
              - sourceSqlStatement: "SELECT * FROM iss_pp_alerts_table where alertid >= ${exchange.properties.checkpointValue}"
                targetSqlStatement: "INSERT INTO dev_alertIds(alertid) VALUES(:?alertid::integer)"
                checkpointKey: "alertid"
                checkpointDefault: "0"
                checkpointName: "besdb_alertid"
```

### Stored Procedure UPSERT example

This example shows how to configure a pipeline where the target database has a stored procedure to handle unique key violations for duplicate data.

You must first create the stored procedure on the target database, which the pipeline call.  

Below is a Postgres example that can be used as a template:

```sql
CREATE OR REPLACE PROCEDURE user-updates_insert (
    _ldate varchar,
    _stime varchar,
    _usersite varchar,
    _users varchar,
    _source varchar,
    _dateimported timestamp
)
LANGUAGE PLPGSQL
AS $$
BEGIN
  INSERT INTO user-updates(ldate, stime, usersite, users, source, dateimported)
    VALUES(_ldate, _stime, _usersite, _users, _source, _dateimported)
  ON CONFLICT (ldate, usersite)
  DO
    UPDATE SET
      stime=_stime,
      users=_users,
      source=_source,
      dateimported=_dateimported;
END;$$
```

Example _user-updates_ pipeline: **config/user-updates.yml**:

```yml
app:
  pipelines:
    "[user-updates]":
      steps:
        - method: pipeline-template
          ref: "db-to-db~1.0"
          properties:
            schedule: "0 * * ? * *"
            triggerName: "bpousers-trigger"
            sourceDataSource: "mssql_db"
            targetDataSource: "dest_db"
            sql:
              - sourceSqlStatement: "SELECT * FROM [test].[dbo].[user-updates]"
                targetSqlStatement: "CALL user-updates_insert(:?ldate::text, :?stime::text, :?usersite::text, :?users::text, :?source::text, :?dateimported::timestamp)"
```

***

## Pipeline Properties

| Property | Default | Description |
|---|---|---|
|***_pipelines: "[xxx]"_** | | Pipeline name - must match the newly created pipeline yaml file|
|***_ref_** | |  Reference to the template including version |
|***_schedule_** | | The cron string that defines the schedule for this pipeline, you can use [this generator][cron]|
|_triggerName_ | *pipeline_name*Trigger | Specify a unique cron trigger name|
|***_sourceDataSource_** || The data source name to use for the pipeline input|
|***_targetDataSource_** || The data source name to use for the pipeline output|
|***_sourceSqlStatement_** || The SQL query that will be executed to read the data for processing|
|***_targetSqlStatement_** || The SQL command that will be executed against each row returned from the _sourceSqlStatement_. It is recommended that you use preparedStatement field definitions to protect against SQL injection, these are specified as `:?fieldname`, see examples|
|_checkpointName_ || The name of the checkpoint to use - this is created dynamically within the pipeline|
|_checkpointKey_ || The name of the field returned in the data returned by the _sourceSqlStatement_
|_checkpointDefault_ || The default value to use for the checkpoint if it cannot be determined |
|_logReceived_|true |If set **true** all messages received will be logged to ```logs/<pipeline name>-<yyyymmdd>.received```|
|_logProcessed_|true |If set **true** all messages processed will be logged to ```logs/<pipeline name>-<yyyymmdd>.processed```|


**_*mandatory property_**

***
[cron]: <http://www.cronmaker.com/>
