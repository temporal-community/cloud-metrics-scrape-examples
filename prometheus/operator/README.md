# Prometheus Operator

The easiest way to get prometheus to scrape Temporal cloud metrics is to use [scrape config](../scrapeconfig/README.md) directly. However, it's possible to leverage the primitives provided by the [prometheus kubernetes operator](https://prometheus-operator.dev/) as well.

One way to do it is by creating a proxy service (using a reverse proxy like nginx) and binding that to a `ServiceMonitor`.  

**Note: one benefit of the proxy service is that it can handle the the auth and just expose an http kubernetes service for easier integration with metrics collectors.**


## Example: nginx proxy + service monitor

## Requirements
* Kubernetes cluster (minikube works as a good local alternative)


Add your Temporal Cloud API Key to [nginx.conf](k8s/nginx.conf) 

This configuration needs to be saved in a secret named `metricsproxy`.

```bash
kubectl create secret generic metricsproxy --from-literal=nginx.conf=$(cat k8s/nginx.conf | base64)
```

Now apply the k8s manifests to create the service, deployment and service monitor

```bash
kubectl apply -f k8s/
```
