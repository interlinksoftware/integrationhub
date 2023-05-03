# webhook-to-tcp v2.2

## Release Notes

| TicketId     | Description |
| ----------- | ----------- |
| IH-454      | Ability to forward the raw payload, in the template use `${raw}` |
| IH-454      | Integration-Hub minimum compatible version is now 2.1.0 |

## Format
```yml
app:
  pipelines:
    "[webhookToBesChannel]":
      steps:
      - method: pipeline-template
        ref: "webhook-to-tcp~2.2"
        properties:
          port: 6781
          destinationServers:
            - hostname: testserver01.interlinksoftware.com
              port:     50555
```

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
|_logFailed_|true |If set **true** all messages that failed to be forwarded to the TCP socket, will be logged to ```logs/<pipeline name>-<yyyymmdd>.failed```|
|_logSuccess_|true |If set **true** all messages that were forwarded to the TCP socket, will be logged to ```logs/<pipeline name>-<yyyymmdd>.success```|
|***_port_**| | Target port number allocated for third party applications (in the example above: 6781)|
|_sslContextParameters_| | The reference to the SSL configuration to use for HTTPS (in the example above: #IssSslConfig)|
|preprocessHeaders| | The headers to be set before processing the recieved payload|
|_templates_| | The transformation applied to the data |
|***_destinationServers - hostname_**| | Hostname where the destination TCP Listener is running|
|***_destinationServers - port_**| | Port number of the destination TCP Listener|
**_*mandatory property_**
***
