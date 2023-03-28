# Telegraftomap Helm chart
## © Copyright 2022 Hewlett Packard Enterprise Development LP

This repository represents the official helm repository of telegraftomap , customized to send data from mosquitto to influxdb complying with the data structure provided by influxdb V2 , for these to be visible through a map on Grafaana   . It is therefore available for download from the [jfrog artifactory](http://134.44.28.237:30238/artifactory/helm-local) as **telegraftomap-helm-0.1.0-official** and derives from the [official telegraf helm chart](https://artifacthub.io/packages/helm/influxdata/telegraf) by [influxdata](https://www.influxdata.com/)

This version like all official helm versions is characterized by the **"official"** flag indicating that this helm package has been ooportunately tested in development environments and therefore is usable and customizable through [argocd](https://argo-cd.readthedocs.io/en/stable/) application or applicationset 

## Prerequisites

- Helm v2 or later
- Kubernetes 1.4+
- (Optional) PersistentVolume (PV) provisioner support in the underlying infrastructure



## Helm Configuration

The following table lists configurable parameters, their descriptions, and their default values stored in `values.yaml`.


| Parameter | Description | Default |
|---|---|---|
| image.repository | Image repository url | telegraf |
| image.tag | Image tag | 1.23.0-alpine |
| image.pullPolicy | Image pull policy | IfNotPresent |
| image.pullSecrets | It will store the repository's credentials to pull image | nil |
| service.enabled | It enables Kubernetes services | false |
| service.type | Kubernetes service type | ClusterIP |
| serviceAccount.create | It will create service account | true |
| serviceAccount.name | Service account name | "" |
| serviceAccount.annotations | Service account annotations | {} |
| serviceAccount.annotations | Service account annotations | {} |
| outputs.influxdb.host | hostname of influxdb where send data   | mosquitto-svc |
| outputs.influxdb.port |port of influxdb where send data  | 30011 |
| outputs.influxdb.bucket | bucket of influxdb where send data  | aq |
| outputs.influxdb.organuzation  | organization of which the influxdb bucket is part of  | HPE |
| inputs.mosquitto.host | hostname of mosquitto on edge  | mosquitto-svc |
| inputs.mosquitto.port | port of mosquitto on edge | 1883 |
| inputs.mosquitt.topics | topic prefix on mosquitto on  where telegraf subscribe to receive data from mosquitto on edge | # |
| inputs.mosquitto.dataFormat | format of messages received from mosquitto on edge   | json |
| inputs.mosquitto.tagKeys |  messages used like tags and  received from mosquitto on edge   | ["lat","lon"] |
| inputs.mosquitto.jsonFields | json Fields accepted from telegraf on mosquitto | ["aqi", "co", "no2", "o3", "pm10", "pm25", "so2","subsystem","timestamp","windspeed","temperature","pressure","humidity"]     |

## Requirements

  requirements indicate the applications that must be present for the application **TelegrafTomap** to work properly
  - [mosquitto](http://gitea.cloud.vm/HELM-PACKAGES/mosquitto-helm)
  - [influxdb > 1.7](http://gitea.cloud.vm/HELM-PACKAGES/influxdb-helm)

# License
### © Copyright 2022 Hewlett Packard Enterprise Development LP