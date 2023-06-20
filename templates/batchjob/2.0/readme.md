# batchjob v2.0 - Pipelines

## Creating Pipeline

Login to your Integration Hub UI. Select the *Templates* menu option and click the  *CREATE PIPELINE* button next to the batchjob template.

<p align="center">
<img src="../../../assets/images/batchjob/create_pipeline.jpg" />
</p>

Name the pipeline accordingly and fill in the properties you'd like to set.

| Property | Default | Description |
|---|---|---|
|***schedule** | | The <a href="https://en.wikipedia.org/wiki/Cron" target="_cron">cron</a> expression that defines the execution schedule for this pipeline|
|***description** | | A description of the batchjob |
|***command** | | The absolute path of the command to be executed |
|args| | Arguments passed to the command (whitespace separated) |
|timeout| 3600000| The number of milliseconds to wait before terminating the command default: 1 hour)|

**_*mandatory property_**
***
<br>

Once you have checked your properties are correct click to *DEPLOY PIPELINE* to create it. 

