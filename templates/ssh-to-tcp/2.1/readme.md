# ssh-to-tcp v2.1

## Release Notes

| TicketId | Description                                                                                                                              |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| IH-467   | Add **resolve-maps-as-key-value** to split definitions to enforce default behaviour                                                      |
| IH-467   | Integration Hub minimum compatability version bumped to `2.1.0`                                                                          |
| IH-467   | Update the netty URL to change `sync=false` to `sync=true` as errors have been observed where the object gets returned to the pool twice |
| IH-437   | Fix issue with SSH component using `certResource` when only username and password have been defined                                      |

## Format

### With Private Key

```yml
app:
  pipelines:
    ssh-to-tcp:
      steps:
        - method: pipeline-template
          ref: ssh-to-tcp
          properties:
            schedule: 0 0/1 * * * ? *
            username: root
            hostname: hostname.domain.com
            port: "22"
            pollCommand: /path/to/command/or/script
            privateKeyAuth: true
            certResource: /path/to/key
            newlinePlaceholder: " | "
            destinationServers:
              - hostname: 127.0.0.1
                port: "<message_channel_port>"
```

### With Standard Authentication

```yml
app:
  pipelines:
    ssh-to-tcp:
      steps:
        - method: pipeline-template
          ref: ssh-to-tcp
          properties:
            schedule: 0 0/1 * * * ? *
            username: root
            hostname: hostname.domain.com
            port: "22"
            pollCommand: /path/to/command/or/script
            password: password
            newlinePlaceholder: " | "
            destinationServers:
              - hostname: 127.0.0.1
                port: "<message_channel_port>"
```

---

## Pipeline Properties

| Property                   | Default       | Description                                                                                                                            |
| -------------------------- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| **\*_pipelines: "[xxx]"_** |               | Pipeline name - must match the newly created pipeline yaml file                                                                        |
| **\*_ref_**                |               | Reference to the template including version                                                                                            |
| _schedule_                 | 0 \* _ ? _ \* | The quartz cron expression that defines the execution schedule for the pipeline                                                        |
| **\*_host_**               |               | The hostname or IP address of the remote SSH server                                                                                    |
| **\*_port_**               |               | The port number of the remote SSH server                                                                                               |
| _certResource_             |               | Full path to the private key (if using private key authentication)                                                                     |
| _certResourcePassword_     |               | The password for the private key (if required)                                                                                         |
| _privateKeyAuth_           | false         | Whether to enable private key authentication or not                                                                                    |
| **\*_username_**           |               | Username to use when connecting to the remote SSH server                                                                               |
| _password_                 |               | Password to use when connecting ot the remote SSH server                                                                               |
| **\*_pollCommand_**        |               | The command string to send to the remote SSH server                                                                                    |
| _timeout_                  | 30000         | The timeout in milliseconds to wait in establishing the remote SSH connection                                                          |
| _blankPlaceholder_         |               | String to replace blank values with                                                                                                    |
| _newlinePlaceholder_       |               | String to replace newline character values with                                                                                        |
| _logDropped_               | true          | If set **true** all dropped messages will be logged to `logs/<pipeline name>-<yyyymmdd>.dropped`                                       |
| _logReceived_              | true          | If set **true** all messages received will be logged to `logs/<pipeline name>-<yyyymmdd>.received`                                     |
| _logProcessed_             | true          | If set **true** all messages processed will be logged to `logs/<pipeline name>-<yyyymmdd>.processed`                                   |
| _logFailed_                | true          | If set **true** all messages that failed to be forwarded to the TCP socket, will be logged to `logs/<pipeline name>-<yyyymmdd>.failed` |
| _logSuccess_               | true          | If set **true** all messages that were forwarded to the TCP socket, will be logged to `logs/<pipeline name>-<yyyymmdd>.success`        |
| **\*_filters_**            |               | The transformation applied to the data and the query used to update the destination database                                           |

**_\*mandatory property_**
