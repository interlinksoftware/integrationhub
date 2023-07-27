# batchjob v2.1 - Pipelines

## Format


```yml
app:
  pipelines:
    "[batch-copy-files]":
      steps:
      - method: pipeline-template
        ref: "batchjob~2.1"
        properties:
          schedule: 0 30 16 1/1 * ? *
          description: "Copy processing files at 4:30pm"
          command: /opt/batch/scripts/copy-files.sh
          args: "rhdev01 rhuat01"
          timeout: "60000"
```

***

## Properties

| Property | Default | Description |
|---|---|---|
|***schedule** | | The cron expression that defines the execution schedule for this pipeline|
|***description** | | A description of the batchjob |
|***command** | | The absolute path of the command to be executed |
|args| | Arguments passed to the command (whitespace separated) |
|timeout| 3600000| The number of milliseconds to wait before terminating the command default: 1 hour)|

**_*mandatory property_**
***
