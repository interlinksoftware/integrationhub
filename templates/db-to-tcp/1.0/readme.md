# db-to-tcp v1.0

## Pipeline Prerequisites

Before creating the pipeline you will need to gather the following information:

- The connection details for the source database, configured in the **datasources.yml** file.
- JDBC compatible drivers and connection properties for the data source installed within integration-hub.
- SQL query to be performed on the source to retrieve the required data.  This can be used with the checkpoint placeholder to avoid reading duplicates (see [the sample pipeline](#create-the-pipeline)).
- A template that is applied after data matches the allowList to transform the data into a format accepted by the TCP Listener (the default template is `+++(${auto}+++` that generates a pipe-delimited string).
- The server and port number for the destination TCP Listener (BES Message Channel).
- Determine the frequency of executing the pipeline, taking into consideration the amount of data being processed and other pipelines executing within the integration-hub instance.

***

### Create TCP BES message channel for TCP related Integration-Hub Pipelines

Where a pipeline feeds alerts to a BES tcp message channel, a message channel may need to be created, which can be done as follows:

- Log onto the BES server as ```ppadmin``` and navigate to the */opt/ISS/utils* directory
- Execute the ```createBesMessageChannel``` utility file to create the BES message channel and when prompted: 
- Enter the message channel name
- Enter a description for the massage channel
- Select TCP from the list of available message channel types
- Enter a port number for the message channel
- Start the message channel: ```ppStart -n <TCP Message Channel Name>```

## Create the Pipeline

Create the pipeline file in the **integration-hub/config** directory.

:bulb: The pipeline file is in YAML format.

Here is a sample that you can use:
```yml
app:
  pipelines:
    "[dbevents-to-bes]":
      steps:
        - method: pipeline-template
          ref: "db-to-tcp~1.0"
          properties:
            logDropped: true
            logReceived: true
            logProcessed: true
            cronSchedule: "0 * * ? * *"
            
            dataSourceName: "events_database"
            sql:
            - selectStatement: 'SELECT * FROM iss_pp_alerts_table where alertid >= ${exchange.properties.checkpointValue};'
              checkpointKey: 'alertid'
              checkpointDefault: '0'
              checkpointName: 'besdb_alertid'

            templates:
              "[${bodyAs(String)} ~~ 'alertid']": "TEMPLATE 1 | ${auto}"
              "[${bodyAs(String)} ~~ 'world']": "TEMPLATE 2 ${body.def}"
              
            destinationServers:
              - hostname: bes-server01.com
                port:     5000
```

***

## Pipeline Properties

| Property | Default | Description |
|---|---|---|
|***_pipelines: "[xxx]"_** | | Pipeline name - must match the newly created pipeline yaml file|
|***_ref_** | |  Reference to the template including version |
|***_cronSchedule_** | | The cron string that defines the schedule for this pipeline, you can use [this generator][cron]|
|***_dataSourceName_** | | The Data source to use for reading input data |
|***_sql_** | | The SQL query that will be executed to read the data for processing from the data source |
|_checkpointName_ | | The name of the checkpoint to use - this is created dynamically within the pipeline|
|_checkpointKey_ | | The name of the field returned in the data returned by the _sql_
|_checkpointDefault_ | 0 | The default value to use for the checkpoint if it cannot be determined |
|_logDropped_|true |If set **true** all dropped messages will be logged to ```logs/<pipeline name>-<yyyymmdd>.dropped```|
|_logReceived_|true |If set **true** all messages received will be logged to ```logs/<pipeline name>-<yyyymmdd>.received```|
|_logProcessed_|true |If set **true** all messages processed will be logged to ```logs/<pipeline name>-<yyyymmdd>.processed```|
|***_templates_**| | The transformation applied to the data and the query used to update the destination database |
|***_destinationServers - hostname_**| | Hostname where the destination TCP Listener is running|
|***_destinationServers - port_**| | Port number of the destination TCP Listener|

**_*mandatory property_**

[cron]: http://www.cronmaker.com/
***
