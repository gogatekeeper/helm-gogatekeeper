# gatekeeper

![Version: 0.1.48](https://img.shields.io/badge/Version-0.1.48-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 2.11.0](https://img.shields.io/badge/AppVersion-2.11.0-informational?style=flat-square)

Gatekeeper is a proxy which integrates with OpenID Connect (OIDC) Providers, it supports both access tokens in a browser cookie or bearer tokens.

## TL;DR

```console
$ helm repo add gogatekeeper https://gogatekeeper.github.io/helm-gogatekeeper
$ helm install my-gatekeeper gogatekeeper/gatekeeper
```

## Prerequisites

* Helm v3.0.0+

## Configuration options

Configuration of Gatekeeper mentioned in the [user-guide.md](https://github.com/gogatekeeper/gatekeeper/blob/master/docs/user-guide.md#configuration-options)
can be defined in your helm values file directly under the key `config:`. Eg.:

```yaml
config:
  discovery-url: <DISCOVERY URL>
  client-id: <CLIENT_ID>
  client-secret: <CLIENT_SECRET>
  upstream-url: http://127.0.0.1:80
  scopes:
  - vpn-user
  resources:
  - uri: /admin/test
  methods:
  - GET
  roles:
  - openvpn:vpn-user
  - openvpn:prod-vpn
  - test
  - uri: /admin/*
  methods:
  - GET
  roles:
  - openvpn:vpn-user
  - openvpn:commons-prod-vpn
```
For the complete list of all available configuration options, please read the
[Configuration Options](https://gogatekeeper.github.io/configuration/) reference.

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` | Affinity settings for pod assignment |
| automountServiceAccountToken | bool | `false` | Enable automounting service account credentials, please be aware that there is more secure way of exposing serviceAccountToken in pods with projected volumes |
| autoscaling.enabled | bool | `false` | Enable autoscaling |
| autoscaling.maxReplicas | int | `100` | Maximum number of Gatekeeper replicas |
| autoscaling.minReplicas | int | `1` | Minimum number of Gatekeeper replicas |
| autoscaling.targetCPUUtilizationPercentage | int | `80` | Target CPU utilization percentage |
| autoscaling.targetMemoryUtilizationPercentage | string | `nil` | Target Memory utilization percentage |
| config | object | See [values.yaml](values.yaml) | Gatekeeper configuration options, see [Configuration options](#configuration-options) section |
| config.enable-metrics | bool | `false` | Setting this enables the prometheus metrics collector at `/oauth/metrics` |
| config.listen | string | `"0.0.0.0:3000"` | the interface definition you wish the proxy to listen, all interfaces is specified as `:<port>`, unix sockets as `unix://<REL_PATH>` or `unix://</ABS PATH>` |
| config.listen-admin | string | `"0.0.0.0:4000"` | port on which metrics and health endpoints will be available, if not specified it will be on above specified port |
| containerSecurityContext | object | See [values.yaml](values.yaml) | Enables container-level security attributes and common container settings |
| extraArgs | list | `[]` | Additional command line arguments to pass to the container |
| extraEnvVars | list | `[]` | Any extra environment variables you would like to pass on to the pod |
| extraVolumeMounts | list | `[]` | Array to add extra mounts |
| extraVolumes | list | `[]` | Array to add extra volumes |
| fullnameOverride | string | `""` | Overrides the full name of the chart |
| hostAliases | list | `[]` |  |
| image.digest | string | `""` | Container image digest sha256:(hash) |
| image.pullPolicy | string | `"IfNotPresent"` | Image pull policy. One of `Always`, `Never`, `IfNotPresent` |
| image.registry | string | `"quay.io"` | Container image registry |
| image.repository | string | `"gogatekeeper/gatekeeper"` | Container image name |
| image.tag | string | `""` | Container image tag (overrides the image tag whose default is the chart appVersion.) |
| imagePullSecrets | list | `[]` | An optional list of references to secrets in the same namespace to use for pulling any of the images used |
| ingress.annotations | object | `{}` | Annotations for Ingress resource |
| ingress.className | string | `""` | The name of the IngressClass cluster resource. (Needs Kubernetes >=1.18) |
| ingress.enabled | bool | `false` | Enable ingress controller resource |
| ingress.hosts | list | See [values.yaml](values.yaml) | A list of host rules used to configure the Ingress |
| ingress.tls | list | `[]` | Ingress TLS configuration |
| kubeVersionOverride | string | `""` | Override the Kubernetes version, which is used to evaluate certain manifests |
| livenessProbe | object | See [values.yaml](values.yaml) | Liveness Probe settings |
| metrics.addPrometheusScrapeAnnotation | bool | `false` | Add scrape Annotations to the pods for Prometheus (when not using PrometheusOperator) |
| metrics.serviceMonitor.additionalLabels | object | `{}` | Additional labels to add to the ServiceMonitor |
| metrics.serviceMonitor.annotations | object | `{}` | Annotations to add to the ServiceMonitor |
| metrics.serviceMonitor.enabled | bool | `false` | Create ServiceMonitor resource for scraping metrics using PrometheusOperator |
| metrics.serviceMonitor.interval | string | `nil` | Interval at which metrics should be scraped |
| metrics.serviceMonitor.namespace | string | `nil` | Specify if the ServiceMonitor will be deployed into a different namespace (blank deploys into same namespace as chart) |
| nameOverride | string | `""` | Overrides the name of the chart |
| nodeSelector | object | `{}` | Node labels for pod assignment |
| pdb.create | bool | `false` | Create Pod disruption budget resource |
| pdb.minAvailable | int | `1` | Number or percentage of pods which must be available during a disruption. |
| podAnnotations | object | `{}` | Annotations for pods |
| podLabels | object | `{}` | Additional labels for pods |
| podSecurityContext | object | See [values.yaml](values.yaml) | Enables pod-level security attributes and common container settings |
| priorityClassName | string | `""` | priorityClassName for the Gatekeeper deployment resource |
| readinessProbe | object | See [values.yaml](values.yaml) | Readiness Probe settings |
| replicaCount | int | `1` | Number of desired pods |
| resources.limits | object | `{}` | Maximum amount of compute resources allowed |
| resources.requests | object | `{}` | Minimum amount of compute resources required |
| service.admin.nodePort | string | `nil` | Service HTTP node port for the admin-only endpoint (live-status, debug, prometheus...) |
| service.admin.port | int | `4000` | Kubernetes Service port for the admin-only endpoint (live-status, debug, prometheus...) |
| service.annotations | object | `{}` | Annotations to add to the Service |
| service.proxy.nodePort | string | `nil` | Service HTTP node port |
| service.proxy.port | int | `3000` | Kubernetes Service port |
| service.type | string | `"ClusterIP"` | Kubernetes Service type |
| serviceAccount.annotations | object | `{}` | Annotations to add to the service account |
| serviceAccount.create | bool | `true` | Specifies whether a service account should be created |
| serviceAccount.name | string | `""` | The name of the service account to use. (If not set and create is true, a name is generated using the fullname template) |
| strategy.type | string | `"Recreate"` | Type of deployment. Can be `Recreate` or `RollingUpdate` |
| tolerations | list | `[]` | Toleration labels for pod assignment |
