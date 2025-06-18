# New Relic Prometheus Agent

New relic provides a helm chart ot run a Prometheus Server in Agent mode, and send metrics to the New Relic Remote Write Endpoint.

See: https://github.com/newrelic/newrelic-prometheus-configurator/blob/main/README.md for more details.

## Example

### Requirements
* Kubernetes cluster (minikube works as a good local alternative)


Create a secret containing your temporal cloud API key
```bash
kubectl create secret generic tcloud-api-key --from-literal=token=<YOUR_API_KEY>
```

Now install the helm chart using your own `values.yaml`:

```bash
helm repo add newrelic-prometheus https://newrelic.github.io/newrelic-prometheus-configurator
helm upgrade --install newrelic newrelic-prometheus/newrelic-prometheus-agent -f values.yaml
```

Now you should start to see data coming into New Relic
