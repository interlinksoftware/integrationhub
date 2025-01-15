# webhook-to-tcp Template

The webhook-to-tcp template provides a secure HTTP endpoint that 3rd Party applications can target to transfer data via a webhook. The integration-hub pipeline transfers, filters/transforms and updates the data, passing it out to the target TCP Listener.

<p align="center">
<img src="../../assets/images/webhook-to-tcp/2.4/flow_webhook-to-tcp.jpg" />
</p>

## Configure

If you wish to forward to an Interlink Software BES Message Channel then you should create a TCP based Message Channel before continuing.

### Create TCP BES message channel for TCP related Integration-Hub Pipelines

Where a pipeline feeds alerts to a BES tcp message channel, a message channel may need to be created, which can be done as follows:

- Log onto the BES server as `ppadmin` and navigate to the _/opt/ISS/utils_ directory
- Execute the `createBesMessageChannel` utility file to create the BES message channel and when prompted:
- Enter the message channel name
- Enter a description for the massage channel
- Select TCP from the list of available message channel types
- Enter a port number for the message channel
- Start the message channel: `ppStart -n <TCP Message Channel Name>`

### Defining an Output Target

**Important:** _The following configuration applies ONLY to syslog-to-tcp versions 2.2+_

This template requires that you have configured an `Output Target`, defining the destination for the processed data.

You can access the Output Targets by clicking on the `Output Targets` navigation item on the navbar on the left.

Here you will be presented with a list of Output Targets that have been configured previously.

<img src="../../assets/images/output_targets.jpg" width="700" />

<br />

To begin creating an Output Target, click the `CREATE OUTPUT TARGET` button located on the top-right of the page.

<img src="../../assets/images/create_output_target.jpg" width="700" />

<br />

Currently, there are two output targets that you can define:

| Type               | Description                                                      |
| :--------------------- | :--------------------------------------------------------------- |
| `TCP`             | Sends the processed message to a TCP listener      |
| `FILE`             | Sends the processed message to a FILE on the local filesystem                                     |

#### TCP Properties

<img src="../../assets/images/create_output_target_tcp.jpg" width="650" />

<br />

| Type               | Description                                                      |
| :--------------------- | :--------------------------------------------------------------- |
| `Output Target Name`             | Name of the Output Target you are creating      |
| `Data Type`             | Sends the processed message to a TCP listener      |
| `Host`             | Hostname or IP address of the TCP listener                                 |
| `Port`             | The port that the TCP listener listen on                                   |

#### FILE Properties

<img src="../../assets/images/create_output_target_file.jpg" width="650" />

<br />

| Type               | Description                                                      |
| :--------------------- | :--------------------------------------------------------------- |
| `Output Target Name`             | Name of the Output Target you are creating     |
| `Data Type`             | Sends the processed message to a TCP listener      |
| `Directory Name`             | The full path to the directory on the local filesystem where the file is located                                 |
| `File Name`             | The name of the file to output the processed message to                                   |
| `Append Newline`             | Toggle to determine whether new messages should be added on a new line or directly appended to the existing text without a line break                                  |

## Migrating from piHTTP to webhook-to-tcp based pipeline

If you wish to migrate from piHTTP to integration-hub, then perform the following steps:

- Ensure that the integration-hub has been installed to the BES server containing the piHTTP integration and that the `piHTTP.cfg` to be converted exists in the _/opt/ISS/POWERpack/cfg_ directory.
- Confirm you have installed the webhook-to-tcp template to the _/opt/ISS/integration-hub/config/templates_ directory.
- Create a TCP based BES Message Channel to receive the messages from the pipeline(s).
- Download the
  `convertBesPihttpToPipeline` from Github, this can be manually downloaded and placed in the **/opt/ISS/integration-hub/config** directory or using the command below as the ppadmin user.
  ```bash
  cd /opt/ISS/integration-hub/config
  curl -k -O https://raw.githubusercontent.com/interlinksoftware/integrationhub/main/templates/webhook-to-tcp/convertBesPihttpToPipeline
  chmod +x convertBesPihttpToPipeline
  ```
- Execute the conversion utility as the ppadmin user and enter both the Fully Qualified Domain Name of the target BES server and the tcp port number of the BES Message Channel created above when prompted.
  ```bash
  cd /opt/ISS/integration-hub/config
  ./convertBesPihttpToPipeline
  ```
  A new pipeline file, `piHTTP.yml`, will be created in the same directory where the `convertBesPihttpToPipeline` file resides for the piHTTP message channel being converted. To activate the converted pipeline:
- Ensure that references to both the webhook-to-tcp template and the converted pipeline .yml files have been added to the **pipelines.yml** file to add them to your integration-hub instance.
- Stop the original piHTTP message channel, if necessary, as the converted piHTTP pipeline will be listening on the original piHTTP message channel port.
- Restart the integration-hub service to pick up the changes.
