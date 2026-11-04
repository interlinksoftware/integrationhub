# piscom-to-tcp Template

The piscom-to-tcp template connects to one or more SCOM (iiSCOM) hosts, receives their alert events, filters and transforms them, and sends the result to a TCP listener, via an integration-hub pipeline.

## Versions

| Version | Minimum Integration Hub version | Documentation |
| ------- | ------------------------------- | ------------- |
| `1.0`   | `3.2.0`                         | [Configuration and usage](1.0/) |

## Output Targets

This template sends its output to Output Targets. See [Defining an Output Target](../syslog-to-tcp#defining-an-output-target) for how to create one.
