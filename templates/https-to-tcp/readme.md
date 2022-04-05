# https-to-tcp Template

The https-to-tcp template provides a secure HTTP endpoint that 3rd Party applications can target to transfer data via a webhook.  The integration-hub pipeline transfers, filters/transforms and updates the data, passing it out to the target TCP Listener.

<p align="center">
<img src="../../assets/images/flow_http-to-tcp.jpg" />
</p>

***

## Install

Download the version of the db-to-tcp template that you require from github to your integration-hub server.

Run the following to install directly from Github:
<font size="1">
```bash
ih-cli template import \
  https://raw.githubusercontent.com/interlinksoftware/integrationhub/main/templates/https-to-tcp/<version>/https-to-tcp~<version>.yml
```
  </font>
  
If your server does not have access to Github you can download the file and place it in the ```integration-hub/config/templates``` directory.
***

## Configure

If you wish to forward to an Interlink Software BES Message Channel then you should create a TCP based Message Channel before continuing.
  
### Create TCP BES message channel for TCP related Integration-Hub Pipelines

Where a pipeline feeds alerts to a BES tcp message channel, a message channel may need to be created, which can be done as follows:

- Log onto the BES server as ```ppadmin``` and navigate to the */opt/ISS/utils* directory
- Execute the ```createBesMessageChannel``` utility file to create the BES message channel and when prompted: 
- Enter the message channel name
- Enter a description for the massage channel
- Select TCP from the list of available message channel types
- Enter a port number for the message channel
- Start the message channel: ```ppStart -n <TCP Message Channel Name>```

### Creating a pipeline

You will need to create a pipeline to use a template. A sample pipeline definition is detailed in the README for each version of the template under the relevant directory, use that as a base for your pipeline.

## Migrating from piHTTP to https-to-tcp based pipeline

If you are wanting to migrate from piHTTP to integration-hub, then perform the following steps:
- Ensure you have installed the https-to-tcp template in the */opt/ISS/integration-hub/config/templates* directory.
- Create a TCP based BES Message Channel to receive the messages from the pipeline(s) on.
- Download the 
  ```convertBesPihttpToPipeline``` from Github, this can be manually downloaded and placed in the **/opt/ISS/integration-hub/config** directory or using the command below as the ppadmin user.
  ```bash
  cd /opt/ISS/integration-hub/config
  curl -k -O https://raw.githubusercontent.com/interlinksoftware/integrationhub/main/templates/https-to-tcp/convertBesPihttpToPipeline
  chmod +x convertBesPihttpToPipeline
  ```
  
- execute the conversion script as the ppadmin user and specify the tcp port number of the BES Message Channel created above.
  ```bash
  cd /opt/ISS/integration-hub/config
  ./convertBesPihttpToPipeline
  ```
- The script will create the pipelines for the piHTTP message channels you have defined. Ensure these have been added to the **pipelines.yml** file to add them to your integration-hub instance.
