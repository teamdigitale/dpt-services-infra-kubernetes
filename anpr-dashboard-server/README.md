# Dashboard app

The dashboard application reads data from some CSV and populates a MongoDB. Data from the MongoDB are then read by [Metabase](https://metabase.com/), which can be used to display statistics and show some charts.

## Requirements

The following steps should be performed before being able to install metabase and the local chart in this folder.

### Namespace

Create an ANPR namespace (if not already present):

```shell
kubectl create namespace anpr
```

### Storage and PVCs

Add the *anpr-dashboard-server-db* and the *anpr-dashboard-server-cache* Persistent Volume Claims (PVCs):

```shell
kubectl apply -f storage/prod-pvc-dashboard-anpr-server.yaml
```

### Create secrets in Azure Keyvault and sync it in Kubernetes

The following secrets need to be created in the Azure Keyvault.

* *k8s-secrets-anpr-dashboard-server-config*

* *k8s-secrets-anpr-dashboard-server-cookie-creds*

* *k8s-secrets-anpr-dashboard-server-email-creds*

* *k8s-secrets-anpr-dashboard-server-oauth-creds*

* *k8s-secrets-anpr-dashboard-server-google-api-key*

For details about the content of each secret, contact other admins.

### Install the charts and its dependencies

The chart will install the ANPR dashboard server and the cronjob that periodically fetches data:

```shell
helm install \
  --namespace anpr \
  anpr-dashboard-server \
  anpr-dashboard-server
```
