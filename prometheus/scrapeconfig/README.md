## Prometheus

Temporal Cloud Metrics can be scraped by prometheus by adding the following scrape config to prometheus' configuration

```yaml
scrape_configs:
  - job_name: temporal-cloud
    static_configs:
      - targets:
        - 'metrics.temporal.io'
    scheme: https
    metrics_path: '/v1/metrics'
    honor_timestamps: true
    authorization:
      type: Bearer
      credentials: 'API_KEY'
```

Validate this is working by querying: `up{job="temporal-cloud"}`
