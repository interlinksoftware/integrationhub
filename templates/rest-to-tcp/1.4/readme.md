<p align="center">
<img src="../../../assets/images/interlink-software.png" />
</p>
<br><br>

# REST to TCP Listener (rest-to-tcp v1.4)

**Important:** _These instructions assume you have Integration Hub v2.5.1+ installed_

- For help installing [Integration Hub](https://docs.interlinksoftware.com/ih/latest/index.html), see the [Installation Guide](https://docs.interlinksoftware.com/ih/latest/install/install_overview.html).

## What's new in rest-to-tcp v1.4

| Enhancement or Feature                                                                           | ID     |
| :----------------------------------------------------------------------------------------------- | :----- |
| Add exception handling for null pointer exceptions                                               | IH-845 |
| Update XML decoding logic when setting the body to account for if the body is a Map or ArrayList | IH-845 |
| Add ability to set the value for `unmarshalXmlAsList` in the uiSchema (Guided Config)            | IH-845 |
| Update uiSchema (Guided Config) to allow the user to configure checkpoint filtering              | IH-845 |
| Update properties in the checkpoint method to account for the new filtering functionality        | IH-845 |
| Extend logic to log dropped messages if `checkpointFilterMatch` returns `false`                  | IH-845 |
| Extend logic to allow for sorting of the incoming message i.e (ASC \| DESC)                      | IH-872 |
| Extend logic to handle and log bad HTTP responses from failed HTTP requests                      | IH-881 |

## Overview

The rest-to-tcp template provides functionality to query, filter/transform and send from HTTP requests to a TCP listener, via an integration-hub pipeline.

## Prerequisites

Before creating the pipeline you will need have the following configured:

- An [Output Target](https://github.com/interlinksoftware/integrationhub/tree/main/templates/rest-to-tcp#defining-an-output-target), defining the destination for the processed data.

- The template is installed and is available within the user interface. Install directly from github or transfer the template to your Integration Hub server.

  - Installing directly from Github:

    ```
    ih-cli template import https://raw.githubusercontent.com/interlinksoftware/integrationhub/main/templates/rest-to-tcp/1.4/rest-to-tcp~1.4.yml
    ```

  - Install from local file. Place the template file in the `integration-hub/config/templates` directory, then run:

    ```
    ih-cli template import <path to template file>
    ```

  **Note:** _You will need to reload the configuration after importing a template before you can use it, to do this run:_

  ```
  ih-cli config reload
  ```

## Configuration

From the Pipelines section of the user interface you can create, update and delete pipelines. The following properties can be set for your pipeline.

<img src="../../../assets/images/rest-to-tcp/1.4/create_pipeline.png" width="800" />

### REST Connection

| Property                  | Description                                     |
| :------------------------ | :---------------------------------------------- |
| `Data Source`             | The dataSource for the connection               |
| `Connection Timeout`      | Duration in milliseconds to timeout the request |
| `Connect Request Timeout` | Duration in seconds to stop the request         |

### Endpoint Configuration

| Property                          | Description                                                                                                                                                                                                                          |
| :-------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Schedule`                        | The cron string used to specify the frequency of requests to each defined endpoint                                                                                                                                                   |
| `Method`                          | Select the HTTP method to use for the REST request                                                                                                                                                                                   |
| `Url Path`                        | Specify the Url path, this will be appended to the selected REST Datasource specified                                                                                                                                                |
| `Request Payload`                 | Enter any data you want to send in the Body of the REST request                                                                                                                                                                      |
| `Format`                          | Expression to use for the transformation of incoming messages                                                                                                                                                                        |
| `Split`                           | Expression to use for splitting incoming data into multiple events                                                                                                                                                                   |
| `Sort Field`                      | The `key` from the incoming message to use for sorting                                                                                                                                                                               |
| `Sort Order`                      | The order of which you want the incoming message to be sorted (asc / desc)                                                                                                                                                           |
| `Numeric Compare`                 | This tells the pipeline whether we are sorting by a number or a regular string                                                                                                                                                       |
| `Pre-processing Headers Override` | Define a collection of expressions and headers for fine-tuning incoming data prior to processing                                                                                                                                     |
| `Checkpoint Enabled`              | Toggle to enable / disable checkpointing                                                                                                                                                                                             |
| `Checkpoint Type`                 | The type of checkpointing to use. For example:<br /><br />`field (value of a key in the body)`<br /><br />OR<br /><br />`date`                                                                                                       |
| `Checkpoint Name`                 | Unique name for checkpointing messages                                                                                                                                                                                               |
| `Checkpoint Key`                  | The key from the payload that you want to set as the checkpoint key.<br /><br />If the checkpoint type is `date` the format would be as follows:<br /><br />`# '1970-01-01', '00:00:00'` <br />`key: "''yyyy-MM-dd'', ''hh:mm:ss''"` |
| `Checkpoint Default`              | Default value to set if the checkpoint does not exist.<br /><br />This can either be a string, or `now` if the checkpoint type is set to `date`                                                                                      |

#### Checkpoint Filtering

Checkpoint filtering can be used to filter incoming messages based on the value currently stored in the checkpoint file. For example, if your checkpoint file stores a date, you can configure checkpoint filtering to only process incoming messages where the incoming date is greater than the stored date in the checkpoint file.

The following fields can be configured for checkpoint filtering:

| Property                   | Description                                                                                                                                             |
| :------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `Enable Checkpoint Filter` | Toggle to enable / disable the checkpoint filter                                                                                                        |
| `Filter Type`              | The type of filtering to use _(date / field)_                                                                                                           |
| `Expression`               | Ths simple expression to use for filter matching<br/><br/>(**_Only required if using the `field` filter type_**)                                        |
| `Date Format`              | The format of the date from the incoming data<br/><br/>For example: `YYYY-MM-DDTHH:mm:ss.sss±HH:MM`<br/><br/>(**_See below for pre-defined examples_**) |

The pre-defined date formats you can set in the `Date Format` fields are as follows:

| Property            | Example                                  | Description                                   |
| :------------------ | :--------------------------------------- | :-------------------------------------------- |
| epoch               | 1751970896000                            | Direct timestamp                              |
| ISO_DATE_TIME       | 2025-07-01T20:45:30+01:00                | Full date-time with offset                    |
| ISO_INSTANT         | 2025-07-01T19:45:30Z                     | Instant in UTC (Z = Zulu time)                |
| ISO_LOCAL_DATE_TIME | 2025-07-01T20:45:30                      | Date-time without offset                      |
| ISO_8601            | 2025-07-01T20:45:30+01:00                | Standard ISO 8601 format (date, time, offset) |
| ISO_ZONED_DATE_TIME | 2025-07-01T20:45:30+01:00[Europe/London] | Date-time with full time zone                 |
| RFC_1123_DATE_TIME  | Tue, 01 Jul 2025 20:45:30 GMT            | Common HTTP/email date format                 |

Below is an example of the config you would set to filter incoming messages using the `ISO_8601` Date Format:

```yaml
checkpointEnabled: true
checkpointType: field
checkpointName: TEST-rest-to-tcp-date
checkpointKey: time
checkpointDefault: "2025-07-01T00:00:00+01:00"
checkpointFilter: true
checkpointFilterType: date
checkpointFilterFormat: ISO_8601
```

### Destination Configuration

The destination configuration specifies where the processed data should be sent. You can choose a single output target or configure multiple targets as needed.

<img src="../../../assets/images/output_target_pipeline_example.jpg" width="600" />

## Additional Settings

### Alert Formatting

<img src="../../../assets/images/rest-to-tcp/1.2/alert_formatting.png" width="700" />

<br/>

| Property              | Description                                                                         |
| :-------------------- | :---------------------------------------------------------------------------------- |
| `Newline Placeholder` | Value to replace newline characters with in the incoming message                    |
| `Blank Placeholder`   | Value to replace blank keys with in the incoming message (defaults to `N/A`)        |
| `Field Limit`         | The maximum number of characters allowed in each field of the incoming message      |
| `Message Limit`       | The maximum number of characters allowed in the message after it has been formatted |

<br/>

### Logging

| Parameter        | Type                                                                                                                                               |
| :--------------- | :------------------------------------------------------------------------------------------------------------------------------------------------- |
| `logReceived`    | If enabled all messages received will be captured, the maximum number of entries is controlled by the `uiMessageLimit` property                    |
| `logDropped`     | If enabled all messages dropped will be captured, the maximum number of entries is controlled by the `uiMessageLimit` property                     |
| `logProcessed`   | If enabled all messages processed will be captured, the maximum number of entries is controlled by the `uiMessageLimit` property                   |
| `logSuccess`     | If enabled all messages that were successfully sent will be captured, the maximum number of entries is controlled by the `uiMessageLimit` property |
| `logFailed`      | If enabled all messages that have failed will be captured, the maximum number of entries is controlled by the `uiMessageLimit` property            |
| `uiMessageLimit` | Specifies the maximum number of messages to store for this pipeline, the default is `200`                                                          |
