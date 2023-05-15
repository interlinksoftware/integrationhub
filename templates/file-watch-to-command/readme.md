# file-watch-to-command Template

The file-watch-to-command template provides functionality to execute a command when a CREATE | MODIFY | DELETE event occurs on a file or files within a specified location

---

## Install

Download the version of the file-watch-to-command template that you require from github to your integration-hub server.

Run the following to install directly from Github:

```bash
ih-cli template import \
  https://raw.githubusercontent.com/interlinksoftware/integrationhub/main/templates/file-watch-to-command/<version>/file-watch-to-command~<version>.yml
```

If your server does not have access to Github you can download the template file and place it in the `integration-hub/config/templates` directory.

---

### Creating a pipeline

You will need to create a pipeline to use a template. A sample pipeline definition is detailed in the README for each version of the template under the relevant directory, use that as a base for your pipeline.
