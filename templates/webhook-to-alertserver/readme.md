# webhook-to-alertserver Template

The webhook-to-alertserver template receives webhook requests (JSON, XML or YAML), filters them, looks up the referenced BES alert and runs actions against it via AlertServer - close, assign, update a field or add a note - plus optional SQL and command actions, via an integration-hub pipeline.

<p align="center">
<img src="../../assets/images/webhook-to-alertserver/flow_webhook-to-alertserver.png" />
</p>

## Versions

| Version | Minimum Integration Hub version | Documentation |
| ------- | ------------------------------- | ------------- |
| `1.0`   | `2.2.0`                         | [Configuration and usage](1.0/) |
| `1.1`   | `3.2.0`                         | [Configuration and usage](1.1/) |
