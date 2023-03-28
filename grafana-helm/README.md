# Grafana Helm Chart 



This repository represents the official helm repository , based on [grafana helm chart](https://github.com/grafana/helm-charts]) of the [grafana:8.4.2-image](https://hub.docker.com/r/grafana/grafana/), and is therefore available for download from the [jfrog artifactory](http://134.44.28.237:30238/artifactory/helm-local) as **grafana-helm:0.1.1-official** . This Helm Chart include the automatic import of customized dashaboards and customized datasources fro our solution.

This version like all official helm versions is characterized by the **"official"** flag indicating that this helm package has been ooportunately tested in development environments and therefore is usable and customizable through [argocd](https://argo-cd.readthedocs.io/en/stable/) application or applicationset

## Prerequisites

- Helm v2 or later
- Kubernetes 1.4+
- (Optional) PersistentVolume (PV) provisioner support in the underlying infrastructure



## Helm Configuration

The following table lists configurable parameters, their descriptions, and their default values stored in `values.yaml`.

  | Parameter            | Description | Default  |
  |--------------------|--------------|------------------------|
    | image.repository | Image repository url | harbor1.cloud.vm/easyiot/grafana-plugins |
| image.tag | Image tag | v4 |
| image.pullPolicy | Image pull policy | Always |
  | service.type | Kubernetes service type of Grafana  | `ClusterIP`       | 
  | service.nodePort  | kubernetes service nodeport of Grafana   | ` `        | 
  | fullnameOverride  | Provide a name to substitute for the full names of prometheus resources   | ` `        |  
  | sidecar.dashboards.enabled  | Enables the cluster wide search for dashboards and adds/updates/deletes them in grafana   | `false' |
     | sidecar.datasources.enabled  | 	Enables the cluster wide search for datasources and adds/updates/deletes them in grafana  | `false' |
    | sidecar.dashboards.searchNamespace | Namespaces list. If specified, the sidecar will search for dashboards config-maps inside these namespaces.Otherwise the namespace in which the sidecar is running will be used.It's also possible to specify ALL to search in all namespaces. | `false' | 
    | sidecar.datasources.searchNamespace | Namespaces list. If specified, the sidecar will search for datasources config-maps inside these namespaces.Otherwise the namespace in which the sidecar is running will be used.It's also possible to specify ALL to search in all namespaces. | `false' |
  | nodeSelector  | select node of cluster where will be deployed the application   | `{} `        | 
  | customized.dashboards.mobileApp.enabled  | it allows the automatic import of the customized dashboard "MobileApp"   | `true `        | 
   | customized.dashboards.forecastingDisplay.enabled  | it allows the automatic import of the customized dashboard "forecasting Display"   | `true `        | 
    | customized.dashboards.monitoring.enabled  | it allows the automatic import of the customized dashboard "monitoring"   | `true `        | 
   | customized.dashboards.map.enabled  | it allows the automatic import of the customized dashboard "Map"   | `true `        | 
   | datasources.InfluxdbV1.enabled  | it allows the automatic import of the  datasource "Influxdb1"   | `true `        | 
     | datasources.InfluxdbV1.url  | it specifies the service url of influxdb1 where to get the metrics from  | `influxdb1:8086' '        |  
     | datasources.InfluxdbV1.dafaultDatabase  | it specifies the predefined database where to get the metrics from   | `Rome"       | 
     | datasources.InfluxdbV2.enabled  | it allows the automatic import of the  datasource "Influxdb2"   | `true `        | 
     | datasources.InfluxdbV2.url  | it specifies the service url  of influxdb2 where to get the metrics from  |   `influxdb1:8086' `        | 
| datasources.InfluxdbV2.defaultBucket  | it specifies the default bucket  of influxdb2 where to get the metrics from  |   `airquality'         | 
  | datasources.InfluxdbV2.defaultOrg  | it specifies the default Organization to which the bucket belongs  |   `HPE'         | 
   | datasources.testdataDB.enabled  | it allows the automatic import of the  datasource "testdataDB"   | `true `        | 
  | datasources.prometheus.enabled  | it allows the automatic import of the  datasource "prometheus"   | `true `        |
  | datasources.prometheus.url  | it specifies the service url  of influxdb2 where to get the metrics from   | `true `        |

   


  To add other Configurations click on [grafana on artifacthub](https://artifacthub.io/packages/helm/grafana/grafana)


## Requirements
  
  requirements indicate the applications that must be present for the application **Grafana** to work properly
  - [prometheus](http://gitea.cloud.vm/HELM-PACKAGES/prometheus-helm)
  - [influxdb ](http://gitea.cloud.vm/HELM-PACKAGES/influxdb-helm)

  
  
  ## Grafana : General Information

  Grafana is a multi-platform open source analytics and interactive visualization web application. It provides charts, graphs, and alerts for the web when connected to supported data sources. A licensed Grafana Enterprise version with additional capabilities is also available as a self-hosted installation or an account on the Grafana Labs cloud service.[2] It is expandable through a plug-in system. End users can create complex monitoring dashboards[3] using interactive query builders. Grafana is divided into a front end and back end, written in TypeScript and Go, respectively.
  As a visualization tool, Grafana is a popular component in monitoring stacks,[5] often used in combination with time series databases such as InfluxDB, Prometheus and Graphite;

in our solution, Grafana is mainly used to depict monitoring and consumption data both cloud and edge side thanks to prometheus, to show an interactive map where all the active subsystems of the Airquality use case are present by connecting with Influxdb 1.8 trough queries written in [Flux](https://docs.influxdata.com/influxdb/v2.0/query-data/get-started/) to fetch data from the bucket  \_airquality\_  and finally to illustrate a dashboard depicting all the metrics analyzed and forecasted by OWL via influxdb 1.8 but with queries written in [InfluxQL](https://docs.influxdata.com/influxdb/v1.8/query_language/) on the various databases denominated as the reference subsystems . 

**Grafana : Automatic Amount of dashabords e datasources**

Grafana using this helm chart enables automatic dashabord and datasource reference importing thanks to a sidecar container. 

 For the dashboards the parameter sidecar.dashboards.enabled is set, a sidecar container is deployed in the grafana pod. This container watches all configmaps (or secrets) in the cluster and filters out the ones with a label as defined in sidecar.dashboards.label. The files defined in those configmaps are written to a folder and accessed by grafana. Changes to the configmaps are monitored and the imported dashboards are deleted/updated.

A recommendation is to use one configmap per dashboard, as a reduction of multiple dashboards inside one configmap is currently not properly mirrored in grafana.
- Example dashboard config:
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: sample-grafana-dashboard
  labels:
     grafana_dashboard: "1"
data:
  k8s-dashboard.json: |-
  [...]
```


For the datasources the parameter sidecar.datasources.enabled is set, an init container is deployed in the grafana pod. This container lists all secrets (or configmaps, though not recommended) in the cluster and filters out the ones with a label as defined in sidecar.datasources.label. The files defined in those secrets are written to a folder and accessed by grafana on startup. Using these yaml files, the data sources in grafana can be imported.
A recommendation is to use one configmap per datasource coupled with a dashabord.

- Example values to add a datasource adapted for Prometheus
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: prometheus-datasource
  labels:
     grafana_datasource: "1"
data:
  prometheus-datasource.yaml: |-
      {
          "apiVersion": 1,
          "datasources": [
            {
              "access":"proxy",
                "editable": true,
                "name": "prometheus",
                "orgId": 1,
                "type": "prometheus",
                "url": "http://prometheus-service.monitoring:8080",
                "version": 1
            } 
          ]
      }
```

# License

### © Copyright 2022 Hewlett Packard Enterprise Development LP

