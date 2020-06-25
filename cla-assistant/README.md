# cla.developers.italia.it

The helm-chart installs the [CLA Assistant](https://cla-assistant.io/) at https://cla.developers.italia.it

## Requirements

The following steps should be performed before being able to install the CLA Assistant.

### Namespace

Create a *cla namespace* (if not already present):

```shell
kubectl create namespace cla
```

### Secret

Create a secret in the Azure keyvault named *k8s-secrets-cla-assistant*, formatted as following

```json
{"mongodb":"YOUR_MONGODB_ADDRESS_WITH_USER_PASS","mongo-initdb-root-username":"YOUR_MONGODB_ROOT_USER","mongo-initdb-root-password":"YOUR_MONGODB_ROOT_PWD","github-client":"YOUR_GH_CLIENT","github-secret":"YOUR_GH_SECRET","github-token":"YOUR_GH_TOKEN"}
```

### Storage and PVCs

Add the *cla-assistant* Persistent Volume Claims (PVCs):

```shell
kubectl apply -f storage/prod-pvc-cla-assistant.yaml
```

### Install the charts and its dependencies

The chart will install the CLA Assistant:

```shell
helm install \
  --namespace cla \
  cla-assistant \
  cla-assistant
```

### MongoDB database backup

A [cronjob](templates/cronjob-backup.yaml) takes care of MongoDB backups. The job saves backups on an Azurefile-based PVC, which can also be browsed from the [Azure GUI](https://portal.azure.com). By default, the backup runs every night at midnight and the last 30 backups are retained. [Default values](values.yaml) can be modified editing the *backup_schedule* and the *daily_backup_retention* variables.
