# Grafana Mimir Helm Chart

This Helm chart deploys a standalone instance of Grafana Mimir, configured to run in non-distributed mode. It provides flexibility for users to customize their Grafana Mimir instance by allowing custom configurations through the `config` field in the `values.yaml` file.

## Features

- **Customizable Configuration:** Add your own Grafana Mimir settings using the `config` field in `values.yaml`.
- **StatefulSet Deployment:** Ensures stability and persistent identity for each Grafana Mimir pod.

## Prerequisites

- Kubernetes 1.20+
- Helm 3.0+

## Installation

Clone the repository. To install the chart:

```bash
helm install <release-name> . -f values.yaml [-f custom-values.yaml]
```

## Uninstallation

To uninstall the chart:

```bash
helm uninstall <release-name>
```

## Configuration

The following table lists the configurable parameters of the chart and their default values:

| Parameter              | Description                                      | Default |
|------------------------|--------------------------------------------------|---------|
| `replicaCount`         | Number of replicas for the StatefulSet          | `1`     |
| `image.repository`     | Grafana Mimir image repository                  | `grafana/mimir` |
| `image.tag`            | Grafana Mimir image tag                         | `latest` |
| `image.pullPolicy`     | Image pull policy                               | `IfNotPresent` |
| `config`               | Custom Grafana Mimir configuration              | `{}`     |
| `resources`            | Resource requests and limits                    | `{}`     |
| `service.type`         | Kubernetes service type                         | `ClusterIP` |
| `service.port`         | Port for accessing Grafana Mimir                | `8080`   |

To override these values, create a `custom-values.yaml` file and provide your own settings:

```yaml
replicaCount: 1
config:
  retention:
    retentionPeriod: 1d
```

Then install the chart with your custom values:

```bash
helm install <release-name> . -f values.yaml -f custom-values.yaml
```

Config file is mounted at `/etc/mimir.yaml`.

## StatefulSet Deployment

This chart uses a `StatefulSet` instead of a `Deployment` to ensure consistent identity and persistent storage for each Grafana Mimir pod. This is essential for stable operation in standalone mode.

## Example

Here's an example `values.yaml` file to deploy Grafana Mimir with custom settings:

```yaml
replicaCount: 1
config:
  - key: "auth_enabled"
    value: "true"
  - key: "metrics_retention"
    value: "30d"
resources:
  requests:
    memory: "2Gi"
    cpu: "1"
  limits:
    memory: "4Gi"
    cpu: "2"
service:
  type: LoadBalancer
```

## Support

For questions or issues with this Helm chart, please open an issue in the chart repository.
