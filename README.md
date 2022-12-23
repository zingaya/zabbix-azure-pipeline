# Zabbix Azure Pipeline

This repository is a perfect example of how to send some data using Zabbix sender, using Azure Pipelines, and setting up a basic template to monitor the status of the [Worldcup 2026 Countdown](https://github.com/zingaya/worldcup2026) build, deployment and retrieve data from the JSON.

## Azure Pipelines

The first thing to note, is that this pipeline is designed to be triggered after the first pipeline finishes.

As I already stated in the [Worldcup 2026 Countdown](https://github.com/zingaya/worldcup2026), this pipeline uses an agent pool, where the agent is installed in the Zabbix server.

Inside the YAML you will see that it does, in a single line BASH command, get the status of the pipelines builds (top 10) and send them using the zabbix-sender.

## Zabbix

Within this repository, there is a template (zabbix-template-pipeline.yaml) for Zabbix ready to import.

The template is designed to be linked to a host, that will serve and monitor the python web app, and the pipelines.

**Python web app** consists of simple items:
- An item as an HTTP Agent (as a master item), to retrieve the entire JSON from the body of the web app.
- And two dependent items (of the master item), to retrieve values using JSON path in the preprocessing rules.

It has 2 simple triggers:
- If the python web app can not get any new data; could be that the script is not working, or it can not reach the host/port.
- If the version for the web app has changed.

**Azure Pipelines** uses a master item, and a LLD rule (low level discovery):
- As for the master item, this is a trapper item. Is the item that will get all JSON data that the Pipeline will send through the zabbix-sender.
- The LLD will discover all pipelines in the project.
- For each of the pipeline discovered, it will create a set of new items
  - 3 dependent items (from the trapper item), will get using JSON path in the preprocessing rules, a value for the last build result, when, and the number of the build.
  - 1 item, as an HTTP Agent, that will continously monitor if the configuration for the pipeline changes

And at last, it has a set of 2 triggers that will create for each pipeline discovered.
- If the configuration has changed
- If the build has failed
