
# Cal Helm Chart

This is a Helm chart for deploying the Cal application on Kubernetes.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.0+

## Dependencies

The chart has a dependency on the PostgreSQL Helm chart. The version of PostgreSQL is specified as "10.3.11", and the repository for the PostgreSQL Helm chart is "https://charts.bitnami.com/bitnami".

## Secrets

The chart uses a Kubernetes Secret to store sensitive information such as the database URL. The Secret is named `calcom-secrets`.

## Ingress

The chart includes support for Ingress. By default, Ingress is disabled. You can enable it by setting `ingress.enabled` to `true` in the `values.yaml` file or by using the `--set` flag.

## Service Account

The chart creates a Service Account for the Cal pods. By default, the Service Account is created. You can disable it by setting `serviceAccount.create` to `false` in the `values.yaml` file or by using the `--set` flag.

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
helm install my-release .
```

## Customizing the Chart Before Installing

You can customize the chart configuration using the `values.yaml` file or the `--set` flag. For example, to set the number of Cal replicas to 3, you can run the following command:

```bash
helm install my-release . --set replicaCount=3
```

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
helm delete my-release
```

## Upgrading the Chart

To upgrade the `my-release` deployment:

```bash
helm upgrade my-release .
```

## Configuration

The following table lists the configurable parameters of the Cal chart and their default values.

| Parameter | Description | Default |
| --------- | ----------- | ------- |
| `image.repository` | Cal image repository | `calcom.docker.scarf.sh/calcom/cal.com` |
| `image.tag` | Cal image tag | `latest` |
| `image.pullPolicy` | Cal image pull policy | `IfNotPresent` |
| `service.type` | Kubernetes Service type | `ClusterIP` |
| `service.port` | Kubernetes Service port | `7777` |
| `pvc.enabled` | Enable persistence using PVC | `True` |
| `pvc.name` | Name of the PVC | `namespace-postgres` |
| `postgresql.enabled` | Enable PostgreSQL | `True` |
| `postgresql.persistence.existingClaim` | Use an existing PVC for PostgreSQL persistence | `namespace-postgres` |
| `hpa.enabled` | Enable horizontal pod autoscaling | `True` |
| `resources` | CPU/Memory resource requests/limits | `{requests: {cpu: '100m', memory: '128Mi'}, limits: {cpu: '200m', memory: '256Mi'}}` |

## Persistence

The chart mounts a Persistent Volume at `/var/lib/postgresql/data`. The volume is created using dynamic volume provisioning. The Persistent Volume Claim is named `namespace-postgres`.

## Resources

The chart sets resource requests and limits for the Cal pods. It requests 100m CPU and 128Mi memory, and sets the CPU limit to 200m and the memory limit to 256Mi.

## Autoscaling

The chart enables horizontal pod autoscaling. The HPA scales the number of pod replicas based on the observed CPU utilization. The target CPU utilization percentage is 50%.
