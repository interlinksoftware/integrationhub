# https-to-tcp v1.0

## Pipeline Prerequisites

Before creating the pipeline you will need to gather the following information:

- The HTTP port number on the Integration Hub server that applications will target to send data.
- A path to create a specific endpoint for applications to hit (Optional).
- A denyList to drop data that matches the specified string(s).  
- An allowList to allow only data that matches the specified string(s).
- A template that is applied after data matches the allowList to transform the data into a format accepted by the TCP Listener.
- The server and port number for the destination TCP Listener (BES Message Channel).  To create the destination TCP Message Channel see ['Create TCP BES Message Channel for TCP related Integration-Hub Pipelines'](https://github.com/interlinksoftware/integrationhub/blob/main/templates/https-to-tcp/readme.md#configure)

***

## Create the Pipeline

Create the pipeline file in the **integration-hub/config** directory.

:bulb: The pipeline file is in YAML format.

Here is a sample pipeline you can use: **integration-hub/config/webhookToBesChannel.yml**:

```yml
app:
  pipelines:
    "[webhookToBesChannel]":
      steps:
      - method: pipeline-template
        ref: "https-to-tcp~1.0"
        properties:
          hostname: 0.0.0.0
          protocol: "https"
          port: 6781
          path: ""
          sslContextParameters: "#IssSslConfig"
          logDropped: true
          logReceived: true
          logProcessed: true
          blankPlaceholder: "N/A"
          newlinePlaceholder: " "
          apiKey: ""
          allowList:
            - "${bodyAs(String)} regex '(?s)(.*?)'" # Allow everything
          denyList:
            - "${bodyAs(String)} ~~ 'bad_event'"
            - "${bodyAs(String)} ~~ 'warn_event'"
          templates:
            '[${bodyAs(String)} ~~ "alertid"]':
              format: "BES_ALERT | ${auto}"
              split: "${body}"
            '[${bodyAs(String)} ~~ "accountId"]':
              format: "USER_ALERT name = ${body.account.accountName!'N/A'} | id = ${body.account.accountId!'N/A'} | dateReceived = ${body.dateReceived!'N/A'} |"
              split: "${body}"
          destinationServers:
            - hostname: testserver01.interlinksoftware.com
              port:     50555
          preprocessHeaders:
            '[${bodyAs(String)} regex "(?s)(.*?)"]':
              'ExampleHeader': "ExampleHeaderValue"
```

To dynamically generate a pipe-delimited string based on the data, use the ${auto} variable.

***

## Pipeline Properties

| Property | Default | Description |
|---|---|---|
|***_pipelines: "[xxx]"_** | | Pipeline name - must match the newly created pipeline yaml file|
|***_ref_** | | Reference to the template including version |
|_blankPlaceholder_ | |  |
|_newlinePlaceholder_ | |  |
|_apiKey_ | |  |
|_logDropped_|true |If set **true** all dropped messages will be logged to ```logs/<pipeline name>-<yyyymmdd>.dropped```|
|_logReceived_|true |If set **true** all messages received will be logged to ```logs/<pipeline name>-<yyyymmdd>.received```|
|_logProcessed_|true |If set **true** all messages processed will be logged to ```logs/<pipeline name>-<yyyymmdd>.processed```|
|***_port_**| | Target port number allocated for third party applications (in the example above: 6781)|
|***_templates_**| | The transformation applied to the data |
|***_destinationServers - hostname_**| | Hostname where the destination TCP Listener is running|
|***_destinationServers - port_**| | Port number of the destination TCP Listener|
|***preprocessHeaders**| | The headers to be set before processing the recieved payload|

**_*mandatory property_**
***
