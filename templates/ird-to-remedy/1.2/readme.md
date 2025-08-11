<p align="center">
<img src="../../../assets/images/interlink-software.png" />
</p>
<br><br>

# BES IRD to Remedy (ird-to-remedy v1.2)

**Important:** _These instructions assume you have Integration Hub v2.2+ installed_

- For help installing [Integration Hub](https://docs.interlinksoftware.com/ih/latest/index.html), see the [Installation Guide](https://docs.interlinksoftware.com/ih/latest/install/install_overview.html).

## Overview

The ird-to-remedy template enables the integration of BES (ird) with BMC Remedy. It allows alert creation, note addition, and closure events within BES to correspondingly create, update, and close incidents or changes within BMC Remedy

## Prerequisites

Before creating the pipeline you will need to have completed / addressed the following:

- Configured a UserAO in ASI that triggers the `createIncident` utility -- More information can be found on our documentation site: [Configure User Automated Operations](https://docs.interlinksoftware.com/asi/2.7.13/configuration/automation/automation.html)

- Configured a Datasource within Integration Hub that connects to your Remedy environment

- Ensured that the template is installed and is available within the user interface. Install directly from github or transfer the template to your Integration Hub server.

  - Installing directly from Github:

    ```
    ih-cli template import https://raw.githubusercontent.com/interlinksoftware/integrationhub/main/templates/ird-to-remedy/1.2/ird-to-remedy~1.2.yml
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

<img src="../../../assets/images/ird-to-remedy/1.2/create_pipeline.png" width="800" />

### Listener Configuration

The pipeline will listen for incoming messages that have been generated via the createIncident utility

The following properties can be specified when configuring the listener:

| Property                | Description                                                       |
| :---------------------- | :---------------------------------------------------------------- |
| `Hostname / IP Address` | The hostname / ip address you want to configure for this listener |
| `Port`                  | The number of the port you wish this pipeline to listen on        |

<br>

> [!IMPORTANT]
> The `Hostname / IP Address` and `Port` need to match what has been configured in your `irdSN.cfg` file i.e:

```bash
config=irdSN.cfg

ServerLocation=localhost
ServerPort=38888
ISSdebug=1

IncidentRetryAttempts=1
ISSassign=1
ISSclose=1
ISSnote=1

alertId=[alertId]
```

### Remedy Connection

| Parameter                 | Type                                                                               |
| :------------------------ | :--------------------------------------------------------------------------------- |
| `Remedy Datasource`       | Datasource that contains the connection details for your Remedy environment        |
| `Incident ID Object Path` | Simple Expression, defining the JSONPath to the Incident ID in the Remedy response |

<br />

#### Global Field Mappings

Use Global Field Overrides to change the value of any fields in the BES alert. For Remedy we reverse the severity as BES and Remedy are the reverse of each other. These overrides are applied to all requests to Remedy before any Local Field Overrides are added.

<img src="../../../assets/images/ird-to-remedy/1.2/global_field_overrides.png" width="800" />

### Create Incident

The properties below define the structure and content of the payload sent to Remedy. Use these to specify which fields to populate and how values are mapped from the incoming alert data.

| Property                 | Description                                                                                                                                                                                                                                                                                          |
| :----------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Fire and Forget`        | When **enabled**, the action is performed in Remedy (such as creating, updating, or closing an incident) but does not update the corresponding alert in BES.<br/><br/>When **disabled**, the action is performed in Remedy and the alert in BES is updated accordingly (e.g., with the incident ID). |
| `Filter`                 | Expressions that can be set to prevent further processing of an incoming alert                                                                                                                                                                                                                       |
| `Path`                   | Remedy endpoint that the payload will be sent to (_default: /api/now/import/u_bes_alert_)                                                                                                                                                                                                            |
| `Create Field Overrides` | Use Create Field Overrides to override the value of any fields in the BES alert when creating an incident                                                                                                                                                                                            |
| `Remedy Payload`         | Define the fields and values you wish to send to Remedy                                                                                                                                                                                                                                              |
| `Response Processing`    | Specify the alert fields and the corresponding values from the Remedy response returned after creating an incident, to update the fields in the BES alert                                                                                                                                            |

### Optional Settings

| Property           | Description                                                                                                                                        |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------- |
| `UI Message Limit` | Limit of failed/dropped/success/processed/received messages to display on the UI                                                                   |
| `logReceived`      | If enabled all messages received will be captured, the maximum number of entries is controlled by the `uiMessageLimit` property                    |
| `logDropped`       | If enabled all messages dropped will be captured, the maximum number of entries is controlled by the `uiMessageLimit` property                     |
| `logProcessed`     | If enabled all messages processed will be captured, the maximum number of entries is controlled by the `uiMessageLimit` property                   |
| `logSuccess`       | If enabled all messages that were successfully sent will be captured, the maximum number of entries is controlled by the `uiMessageLimit` property |
| `logFailed`        | If enabled all messages that have failed will be captured, the maximum number of entries is controlled by the `uiMessageLimit` property            |
