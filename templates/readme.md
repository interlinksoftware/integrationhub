# Integration Hub Templates

The Integration Hub Templates provide standardized configurations for integrating with third-party databases, systems and applications. Templates drive a consistent approach to the orchestration of pipelines for processing data received into the Interlink AIOps platform.

## Webhooks / HTTP(s) requests

The following templates can be used within your pipelines to process HTTP(s) requests and webhooks, go to the individual template for details on configuration options.

* [HTTP(s) to TCP socket](https-to-tcp) template receives webhook(s)/HTTP POST requests, filters and processes the data into a template as defined in the properties. Supported data formats are JSON, XML and YML.
* [HTTP(s) to Database](https-to-db) template enables CRUD operations against a relational database. This is particularly helpful if you want to create a REST endpoint for accessing and managing data. Any JDBC compliant database is supported.

## Databases

The following templates can be used within your pipelines to process data from Databases, go to the individual template for details on configuration options.

* [Database to TCP socket](db-to-tcp) template executes queries based on a defined schedule, filtering and processing the data into a template as defined in the properties. Any JDBC compliant database is supported.
* [Database to Database](db-to-db) template enables copying of data between databases on a defined schedule. This can be used to consolidate data from federated datasources. Any JDBC compliant database is supported.

***
