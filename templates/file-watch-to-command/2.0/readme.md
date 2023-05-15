# file-watch-to-command v2.0

## Release Notes

| TicketId | Description |
| -------- | ----------- |

## Format

```yml
app:
  pipelines:
    file-watch-to-command-pipeline:
      steps:
        - method: pipeline-template
          ref: file-watch-to-command
          properties:
            path: /path/to/file/or/directory
            autoCreate: true
            command: /command/or/script/to/execute
            arguments: <arguments_to_pass_to_the_script_or_command>
            events: <Events to listen for (CREATE | MODIFY | DELETE)>
```

---

## Pipeline Properties

| Property                   | Default | Description                                                                                                              |
| -------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------ |
| **\*_pipelines: "[xxx]"_** |         | Pipeline name - must match the newly created pipeline yaml file                                                          |
| **\*_ref_**                |         | Reference to the template including version                                                                              |
| **\*_path_**               |         | The path to the file that you wish to listen for modification events on                                                  |
| _antInclude_               | \*\*    | ANT style pattern to match files. **Pattern must be relative (not starting with slash)**                                 |
| _autoCreate_               | false   | Set to **true** if you want the pipeline to automatically create the directory specified in **path**                     |
| _events_                   | CREATE  | Comma separated list of events to listen for i.e - (CREATE, MODIFY, DELETE)                                              |
| **\*_command_**            |         | The full path to the command or script you wish to be executed once an event is detected                                 |
| _arguments_                |         | Arguments that you wish to pass to the command or script                                                                 |
| _timeout_                  | 3600000 | The number in milliseconds to wait before terminating the command (**default: 1 hour**)                                  |
| _logProcessed_             | true    | To log the data once processed into its final form. _**The received file is logs/<pipeline name>-<yyyymmdd>.processed**_ |
| _logFailed_                | true    | To log all failed data. _The failed log file is **logs/<pipeline name>-<yyyymmdd>.failed**_                              |
| _uiMessageLimit_           | 200     | Limit of failed/success/processed/received messages to display in the UI                                                 |

**_\*mandatory property_**
