# Datadog

We use Datadog to monitor our cluster. To do so, a Datadog agent needs to be installed on top of Kubernetes.

The Datadog agent can be installed using its official helm-chart.

## Prerequisites

### Create the namespace for Datadog

```shell
kubectl create namespace datadog-monitoring
```

### Create secret in the Azure Keyvault

Create a secret in the Azure keyvault called *k8s-secrets-datadog-monitoring*, defined as follows:

```json
{ "api-key": "YOUR_DATADOG_API_KEY", "app-key": "YOUR_DATADOG_APP_KEY" }
```

> NOTE: The Datadog API key and APP key should be retrieved from the Datadog website.

Now, sync the Azure secret with the local Kubernetes secrets.

```shell
kubectl apply -f secrets-prod-datadog-monitoring.yaml
```

## Installation

You can now proceed with the Datadog helm-chart installation.

```shell
helm upgrade \
    --install \
    --set datadog.apiKeyExistingSecret=datadog-monitoring-secrets \
    --set datadog.appKeyExistingSecret=datadog-monitoring-secrets \
    --set datadog.site=datadoghq.eu \
    --set clusterAgent.enabled=true \
    --set clusterAgent.metricsProvider.enabled=true \
    --set logsEnabled=true \
    --set logsConfigContainerCollectAll=true \
    --recreate-pods \
    --version 1.39.5 \
    --namespace datadog-monitoring \
    datadog-monitoring \
    stable/datadog
```
