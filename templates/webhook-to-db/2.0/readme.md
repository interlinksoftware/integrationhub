<p align="center">
<img src="../../../assets/images/interlink-software.png" />
</p>
<br><br>

# Webhook to DB (webhook-to-db v2.0)

**Important:** _These instructions assume you have Integration Hub v2.0+ installed_

- For help installing [Integration Hub](https://docs.interlinksoftware.com/ih/latest/index.html), see the [Installation Guide](https://docs.interlinksoftware.com/ih/latest/install/install_overview.html).

## Overview

The webhook-to-db template provides a secure HTTP endpoint that 3rd Party applications can target to transfer data via a webhook. The integration-hub pipeline transfers, filters/transforms and updates the data, sending it on to the target database using JDBC.

## Prerequisites

Before creating the pipeline you will need have the following configured:

- The template is installed and is available within the user interface. Install directly from github or transfer the template to your Integration Hub server.

  - Installing directly from Github:

    ```
    ih-cli template import https://raw.githubusercontent.com/interlinksoftware/integrationhub/main/templates/webhook-to-db/2.0/webhook-to-db~2.0.yml
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

### Webhook Listener

| Parameter        | Type                                                       |
| :--------------- | :--------------------------------------------------------- |
| `host`           | Host to bind the listener to                               |
| `port`           | Port to bind the listner to                                |
| `protocol`       | The HTTP protcol for the HTTP listner (HTTP \| HTTPS)      |
| `dataSourceName` | The name of the data source to use for the pipeline output |
| `templates`      | List of templates to match sql statements against          |

### Data Source

| Parameter        | Type                                                       |
| :--------------- | :--------------------------------------------------------- |
| `dataSourceName` | The name of the data source to use for the pipeline output |
| `templates`      | List of templates to match sql statements against          |

## Format

```yml
app:
  pipelines:
    "sample-pipeline":
      steps:
        - method: pipeline-template
          ref: "webhook-to-db~2.0"
          properties:
            host: 0.0.0.0
            port: 6767
            protocol: "HTTPS"
            dataSourceName: "besdb"
            templates:
              "[${header.CamelHttpMethod} in 'POST' && ${header.table} == 'iss_es_users_table' ]": "INSERT INTO ${headers.table}(username, allowlogin, alertclose, alertassign) values(:?username, :?allowlogin::smallint, :?alertclose::smallint, :?alertassign::smallint);"
```

## Logging

| Parameter      | Type                                                                 |
| :------------- | :------------------------------------------------------------------- |
| `logReceived`  | If enabled all messages received will be captured                    |
| `logProcessed` | If enabled all messages processed will be captured                   |
| `logSuccess`   | If enabled all messages that were successfully sent will be captured |
| `logFailed`    | If enabled all messages that have failed will be captured            |
