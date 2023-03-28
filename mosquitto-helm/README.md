# Mosquitto Helm Chart


This repository represents the unofficial helm repository of the [mosquitto image](https://mosquitto.org/) and is therefore available for download from the [jfrog artifactory](http://134.44.28.237:30238/artifactory/helm-local) as **mosquitto:0.1.0-official** 

This version like all official helm versions is characterized by the **"official"** flag indicating that this helm package has been ooportunately tested in development environments and therefore is usable and customizable through [argocd](https://argo-cd.readthedocs.io/en/stable/) application or applicationset 


## Prerequisites

- Helm v2 or later
- Kubernetes 1.4+
- (Optional) PersistentVolume (PV) provisioner support in the underlying infrastructure



## Helm Configuration

The following table lists configurable parameters, their descriptions, and their default values stored in `values.yaml`.


| Parameter | Description | Default |
|---|---|---|
| image.repository | Image repository url |  eclipse-mosquitto |
| image.tag | Image tag | 2.0.14 |
| image.pullPolicy | Image pull policy | IfNotPresent |
| image.pullSecrets | It will store the repository's credentials to pull image | nil |
| service.enabled | It enables Kubernetes services | false |
| service.type | Kubernetes service type | ClusterIP |
| service.port | Kubernetes service port | 1883 |
| nodeSelector  | selector of Kubernetes  worker node  | {} |
| serviceAccount.create | It will create service account | true |
| serviceAccount.name | Service account name | "" |
| serviceAccount.annotations | Service account annotations | {} |
| serviceAccount.annotations | Service account annotations | {} |

**Mosquitto: General Information**

Mosquitto is an open source  message broker that implements the MQTT protocol versions 5.0, 3.1.1 and 3.1. Mosquitto is lightweight and is suitable for use on all devices from low power single board computers to full servers.

The MQTT protocol provides a lightweight method of carrying out messaging using a publish/subscribe model. This makes it suitable for Internet of Things messaging such as with low power sensors or mobile devices such as phones, embedded computers or microcontrollers.

**Mosquitto : Principal commands**

- subscription to  specific a topic
```
mosquitto_sub -h localhost -t <YourTopic>
```
- subscription to  all topics

```
mosquitto_sub -v -h localhost -p 1883 -t '#'
```

- Publishing a message to a topic

```
mosquitto_pub -h localhost -t <YourTopic> -m <YourMessage>
```

- Publishing a message to all topics

```
mosquitto_pub -h localhost -t '#' -m <YourMessage>
```

For other Information on commands click on [mosquitto-commands](http://www.steves-internet-guide.com/mosquitto_pub-sub-clients/)



# License

### © Copyright 2022 Hewlett Packard Enterprise Development LP
