# azure-eventhubs-to-tcp v2.2

## Release Notes

| TicketId | Description                                                         |
| -------- | ------------------------------------------------------------------- |
| IH-482   | Introduce the ability to connect to Azure Eventhubs through a proxy |

## Format

### Without Proxy

```yml
app:
  pipelines:
    azure-eventhubs-to-tcp-no-proxy:
      steps:
        - method: pipeline-template
          ref: azure-eventhubs-to-tcp~2.2
          properties:
            namespace: <Event Hubs Namespace>
            eventHubName: <Event Hubs Name>
            sharedAccessName: <Shared Access Name for your Event Hubs environment>
            sharedAccessKey: <Access Key for your Event Hubs environment>
            blobAccountName: <The account name for the blob storage in azure>
            blobAccessKey: <The access key for the blob storage>
            blobContainerName: <The container name for the blob storage>
            proxyEnabled: false
            destinationServers:
              - hostname: 127.0.0.1
                port: "45768"
```

### With Proxy

```yml
app:
  pipelines:
    azure-eventhubs-to-tcp-with-proxy:
      steps:
        - method: pipeline-template
          ref: azure-eventhubs-to-tcp~2.2
          properties:
            namespace: <Event Hubs Namespace>
            eventHubName: <Event Hubs Name>
            sharedAccessName: <Shared Access Name for your Event Hubs environment
            sharedAccessKey: <Access Key for your Event Hubs environment>
            blobAccountName: <The account name for the blob storage in azure>
            blobAccessKey: <The access key for the blob storage>
            blobContainerName: <The container name for the blob storage>
            proxyEnabled: true
            proxyHostname: <Hostname / IP address of proxy server>
            proxyPort: "<Port that the proxy server is listening on>"
            destinationServers:
              - hostname: 127.0.0.1
                port: "45768"
```

---

## Pipeline Properties

| Property                   | Default | Description                                                                                                                                             |
| -------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **\*_pipelines: "[xxx]"_** |         | Pipeline name - must match the newly created pipeline yaml file                                                                                         |
| **\*_ref_**                |         | Reference to the template including version                                                                                                             |
| **\*_namespace_**          |         | The Eventhubs namespace in Azure                                                                                                                        |
| **\*_eventHubName_**       |         | The EventHubs name under the selected namespace                                                                                                         |
| **\*_sharedAccessName_**   |         | The name you chose when configuring the EventHubs SAS keys                                                                                              |
| **\*_sharedAccessKey_**    |         | The generated key for the _sharedAccessName_                                                                                                            |
| **\*_blobAccountName_**    |         | The account name of the blob storage account to use for checkpointing                                                                                   |
| **\*_blobAccessKey_**      |         | The access key associated with the blob storage account                                                                                                 |
| **\*_blobContainerName_**  |         | The container name created under the blob storage account                                                                                               |
| _proxyEnabled_             | false   | Set to 'true' if you wish to connect to EventHubs through a proxy                                                                                       |
| _proxyHostname_            |         | Hostname/IP address of the proxy server                                                                                                                 |
| _proxyPort_                |         | Port that the proxy server is listening on                                                                                                              |
| _proxyType_                | HTTP    | The protocol that the proxy will use i.e - (DIRECT \| HTTP \| SOCKS)                                                                                    |
| _proxyAuthenticationType_  | NONE    | The authentication method that will be used to authenticate against the proxy i.e - (NONE \| BASIC \| DIGEST)                                           |
| _proxyUsername_            |         | Username to authenticate against the proxy with (if **'_proxyAuthenticationType_'** is enabled)                                                         |
| _proxyPassword_            |         | Password to authenticate against the proxy with (if **'_proxyAuthenticationType_'** is enabled)                                                         |
| **\*_destinationServers_** |         | List of servers to send the pipeline output to                                                                                                          |
| _blankPlaceholder_         |         | String to replace blank values with                                                                                                                     |
| _newlinePlaceholder_       |         | String to replace newline characters values with                                                                                                        |
| _preprocessHeaders_        | NOT_SET |                                                                                                                                                         |
| _allowList_                |         | List of logic statements to determine if the request can proceed                                                                                        |
| _denyList_                 |         | List of logic statements to determine if the request should be halted                                                                                   |
| _filters_                  |         | List of filters to match and format the data                                                                                                            |
| _logProcessed_             | true    | To log the data once processed into its final form. _**The received file is logs/<pipeline name>-<yyyymmdd>.processed**_                                |
| _logSuccess_               | true    | To log messages that were successfully sent to the tcp destination, set this to true. _**The success file is logs/<pipeline name>-<yyyymmdd>.success**_ |
| _logReceived_              | true    | To log all received data, set this to true. _**The received file is logs/<pipeline name>-<yyyymmdd>.received**_                                         |
| _logDropped_               | true    | To log all dropped data, set this to true. _**The dropped file is logs/<pipeline name>-<yyyymmdd>.dropped**_                                            |
| _logFailed_                | true    | To log all failed data. _The failed log file is **logs/<pipeline name>-<yyyymmdd>.failed**_                                                             |
| _uiMessageLimit_           | 200     | Limit of failed/success/processed/received messages to display in the UI                                                                                |

**_\*mandatory property_**
