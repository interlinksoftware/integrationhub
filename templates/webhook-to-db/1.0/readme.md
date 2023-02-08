# webhook-to-db v1.0

## Pipeline Prerequisites

Before creating the pipeline you will need to gather the following information:

- The HTTP port number on the Integration Hub server that applications will target to send data.
- A denyList to drop data that matches the specified string(s).
- An allowList to allow only data that matches the specified string(s).
- The connection details for the destination data source, configured in the **datasources.yml** file.
- JDBC compatible drivers and connection properties for the data source installed within integration-hub.
- SQL update/insert to be performed on the destination data source.  This can be a procedure or function depending on the complexity of the update.

***

## Create the Pipeline

Create the pipeline file in the **integration-hub/config** directory.

:bulb: The pipeline file is in YAML format.

The templates formatting in the pipeline contains the SQL statement(s) that will be executed from the webhook. For more complex updates it is recommended that stored procedures are used.

Below is an example of a pipeline that will add records to a database from the webhooks received:
```yml
app:
  pipelines:
    "[sample-pipeline]":
      steps:
        - method: pipeline-template
          ref: "webhook-to-db~1.0"
          properties:
            logDropped: true
            logReceived: true
            logProcessed: true
            sslContextParameters: "#sslConfig"
            protocol: "https"
            host: 0.0.0.0
            port: 6767
            dataSourceName: "besdb"
            templates:
              "[${header.CamelHttpMethod} in 'POST' && ${header.table} == 'iss_es_users_table' ]": "INSERT INTO ${headers.table}(username, allowlogin, alertclose, alertassign) values(:?username, :?allowlogin::smallint, :?alertclose::smallint, :?alertassign::smallint);"
```

***

## Pipeline Properties

| Property | Default | Description |
|---|---|---|
|***_pipelines: "[xxx]"_** | | Pipeline name - must match the newly created pipeline yaml file|
|***_ref_** | |  Reference to the template including version |
|_blankPlaceholder_ | |  |
|_newlinePlaceholder_ | |  |
|_apiKey_ | |  |
|_logDropped_|true |If set **true** all dropped messages will be logged to ```logs/<pipeline name>-<yyyymmdd>.dropped```|
|_logReceived_|true |If set **true** all messages received will be logged to ```logs/<pipeline name>-<yyyymmdd>.received```|
|_logProcessed_|true |If set **true** all messages processed will be logged to ```logs/<pipeline name>-<yyyymmdd>.processed```|
|***_port_**| | Target port number allocated for third party applications|
|***_dataSourceName_**| | Target database |
|***_templates_**| | The transformation applied to the data and the query used to update the destination database |

**_*mandatory property_**
