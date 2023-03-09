# webhook-to-db v2.0

## Format
```yml
app:
  pipelines:
    "[sample-pipeline]":
      steps:
        - method: pipeline-template
          ref: "webhook-to-db~1.0"
          properties:
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
|_logFailed_|true |If set **true** all messages that failed to be forwarded to the TCP socket, will be logged to ```logs/<pipeline name>-<yyyymmdd>.failed```|
|_logSuccess_|true |If set **true** all messages that were forwarded to the TCP socket, will be logged to ```logs/<pipeline name>-<yyyymmdd>.success```|
|***_port_**| | Target port number allocated for third party applications|
|***_dataSourceName_**| | Target database |
|***_templates_**| | The transformation applied to the data and the query used to update the destination database |

**_*mandatory property_**
