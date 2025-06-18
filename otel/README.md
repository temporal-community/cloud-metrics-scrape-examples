# OpenTelemetry Collector

## Sample Config

```yaml
receivers:
    prometheus:
    config:
        scrape_configs:
        - job_name: 'temporal-cloud'
            static_configs:
            - targets:
                - "metrics.temporal.io"
            scheme: https
            metrics_path: /v1/metrics
            honor_timestamps: true
            authorization:
            type: Bearer
            credentials_file: <API_KEY_FILE>

processors:
    batch:

exporters:
    otlphttp:
    endpoint: <ENDPOINT>

service:
    pipelines:
    metrics:
        receivers: [prometheus]
        processors: [batch]
        exporters: [otlphttp]

```

## Example: TemporalCloud -> NewRelic via OTLP

This simple example demonstrates monitoring prometheus sources with the [OpenTelemetry collector](https://opentelemetry.io/docs/collector/), using the [prometheus receiver](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/prometheusreceiver) and sending the data to New Relic via OTLP (However, any OTLP receiver can be used in place of this)

### Requirements
* Kubernetes cluster (minikube works as a good local alternative)
* Update [secrets.yaml](k8s/secrets.yaml) with your new relic license key (should be able to create an account with a free trial)


Create a secret containing your temporal cloud API key
```bash
kubectl create secret generic tcloud-api-key --from-literal=token=<YOUR_API_KEY>
```


Apply the k8s files to your cluster to run this example:

```bash
kubectl apply -f k8s/
```


### Viewing your data

To review your statsd data in New Relic, navigate to "New Relic -> Query Your Data". To list the metrics reported, query for:

```
FROM Metric SELECT uniques(metricName) WHERE otel.library.name = 'otelcol/prometheusreceiver' LIMIT MAX
```

See [get started with querying](https://docs.newrelic.com/docs/query-your-data/explore-query-data/get-started/introduction-querying-new-relic-data/) for additional details on querying data in New Relic.
