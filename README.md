# Zabbix Azure Pipeline

This repository is a perfect example of how to send some data using Zabbix sender, using Azure Pipelines, and setting up a basic template to monitor the status of the [Worldcup 2026 Countdown](https://github.com/zingaya/worldcup2026) build, deployment and retrieve data from the JSON.

## Azure Pipelines

The first thing to note, is that this pipeline is designed to be triggered after the first pipeline finishes.

As already stated on the other repository, this pipeline uses an agent pool, where the agent is installed in the Zabbix server.

Inside the YAML you will see that it does, in a single line BASH command, get the status of the pipelines builds (top 10) and send them using the zabbix-sender.\

## Zabbix

A zabbix item type as trapper, will be ready to receive any data.
