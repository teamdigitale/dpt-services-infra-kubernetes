# certs.anpr.italia.it

The instance of Nginx allows ANPR users to download test certificates from https://certs.anpr.italia.it

## Requirements

The following steps should be performed before being able to install metabase and the local chart in this folder.

### Namespace

Create an ANPR namespace (if not already present):

```shell
kubectl create namespace anpr
```

### Storage and PVCs

Add the *certs-anpr-italia-it* Persistent Volume Claims (PVC):

```shell
kubectl apply -f storage/prod-pvc-certs-anpr-italia-it.yaml
```

### Install the charts and its dependencies

The chart will install the ANPR dashboard server and the cronjob that periodically fetches data:

```shell
helm install \
  --namespace anpr \
  certs-anpr-italia-it \
  certs-anpr-italia-it
```
