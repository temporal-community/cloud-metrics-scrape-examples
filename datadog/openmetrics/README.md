# Datadog Open Metrics Integration

Temporal Cloud metrics can be scraped by Datadog using the [OpenMetrics](https://docs.datadoghq.com/integrations/openmetrics/) integration. 

## Local Deployment

Install the datadog agent for the platform of your choice.
https://docs.datadoghq.com/agent/basic_agent_usage/

e.g. macos

```bash
DD_API_KEY=<DATADOG_API_KEY> DD_SITE="datadoghq.com" bash -c "$(curl -L https://install.datadoghq.com/scripts/install_mac_os.sh)"
```

Now the agent needs to be configured to run the openmetrics check so edit the configuration (on macos this at `~/.datadog-agent/conf.d/openmetrics.d/conf.yaml`)
to look like the following

```yaml
init_config:
    timeout: 60
    service: temporalcloud

instances:
  - openmetrics_endpoint: https://metrics.temporal.io/v1/metrics
    metrics:
    - .*
    tag_by_endpoint: false
    headers:
      Authorization: "Bearer <TEMPORAL_CLOUD_API_KEY>"
    min_collection_interval: 60
```

Restart the agent so it picks up the configuration change:

```bash
datadog-agent stop
datadog-agent run
```

Within a minute you should start seeing temporal cloud metrics in datadog

