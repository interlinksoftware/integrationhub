# Integration Hub Templates

The Integration Hub Templates provide standardized configurations for integrating with third-party databases, systems and applications. Templates drive a consistent approach to the orchestration of pipelines for processing data received into the Interlink AIOps platform.

## Webhooks

The following templates can be used within your pipelines to process HTTP(s) requests and webhooks, go to the individual template for details on configuration options.

* [Webhook to TCP socket](webhook-to-tcp) template receives webhook requests, filters and processes the data into a template as defined in the properties. Supported data formats are JSON, XML and YML.
* [Webhook to Database](webhook-to-db) template enables CRUD operations against a relational database. This is particularly helpful if you want to create a REST endpoint for accessing and managing data. Any JDBC compliant database is supported.

## Webhooks response status

The table below documents the possible HTTP response status values and their meaning.

|HTTP Status|Description|
| ----------- | ----------- |
|200|Request successfully processed|
|206|Request partially processed, 1 or more operations did not succeed|
|400|Invalid request – unable to process request payload
|401|Unauthorised – incorrect username / password
|500|Internal server error – all operations from the request failed

## Databases

The following templates can be used within your pipelines to process data from Databases, go to the individual template for details on configuration options.

* [Database to TCP socket](db-to-tcp) template executes queries based on a defined schedule, filtering and processing the data into a template as defined in the properties. Any JDBC compliant database is supported.
* [Database to Database](db-to-db) template enables copying of data between databases on a defined schedule. This can be used to consolidate data from federated datasources. Any JDBC compliant database is supported.

***

## Batchjob

* [Batchjob](batchjob) template allows the execution of non-interactive commands and scripts against a schedule. 

