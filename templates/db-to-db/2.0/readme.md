# db-to-db v2.0

## Release Notes

| TicketId     | Description |
| ----------- | ----------- |


## Format
```yml
app:
  pipelines:
    alerts-sync:
      steps:
      - method: pipeline-template
        ref: db-to-db~2.0
        properties:
          sql:
          - sourceSqlStatement: select alertid, eid, freetext from iss_alerts_table
            targetSqlStatement: call proc_alerts(:?alertid::integer, :?eid::integer, :?freetext::text)
          schedule: 0 0/10 * * * ? *
          sourceDataSource: besdb
          targetDataSource: targetdb

```

***

## Pipeline Properties

| Property | Default | Description |
|---|---|---|
|***_pipelines: "[xxx]"_** | | Pipeline name - must match the newly created pipeline yaml file|
|***_ref_** | | Reference to the template including version |
|***_sql_** | | Array of sourceSqlStatement and targetSqlStatement to be executed |
|***schedule** | | The cron expression that defines the execution schedule for this pipeline|
|***_sourceDataSource_*** | | The datasource to be used for running the sourceSqlStatement(s) |
|***_targetDataSource_*** | | The datasource to be used for running the destinationSqlStatement(s) |
|_checkpointEnabled_|false |If set **true** the checkpointing will be recorded|
|_checkpointName_| |A unique name for this checkpoint field|
|_checkpointKey_| |The field returned from the SQL source statement whose value is stored|
|_checkpointType_| |The SQL data type (ie: date, integer, text)|
|_checkpointDefault_| |A default value to used when the piepline is first run or if a value cannot be determined|
|_logReceived_|true |If set **true** all messages received will be logged to ```logs/<pipeline name>-<yyyymmdd>.received```|
|_logProcessed_|true |If set **true** all messages processed will be logged to ```logs/<pipeline name>-<yyyymmdd>.processed```|
|_logFailed_|true |If set **true** all messages that failed to be forwarded to the TCP socket, will be logged to ```logs/<pipeline name>-<yyyymmdd>.failed```|
|_logSent_|true |If set **true** all messages that were forwarded to the TCP socket, will be logged to ```logs/<pipeline name>-<yyyymmdd>.sent```|
|_uiMessageLimit_|200 |Limit of log messages to display on the UI.|
**_*mandatory property_**
***
