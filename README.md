# NATS Sample Helm Chart

This Helm chart is designed for efficient and manageable deployments of a NATS cluster within Kubernetes. It includes configurations for high availability, performance tuning, and observability with Prometheus integration. Users should adapt the default values according to their cluster workload requirements and resource availability.

## Installation

```sh
helm upgrade --install myrelease helm
```

## Configuration

You can customize your NATS cluster via the `values.yaml` file. The following table describes the configurable parameters in the `values.yaml` file of the NATS Helm chart and their default values.

| Parameter                              | Description                                                     | Default Value                         |
| -------------------------------------- | --------------------------------------------------------------- | ------------------------------------- |
| `service.loadBalancer.enabled`         | Enables a LoadBalancer to expose the NATS service               | `true`                                |
| `nats.global.labels.app`               | Global label applied to all NATS components                     | `mynats`                              |
| `nats.natsBox.enabled`                 | Enables natsBox, an auxiliary container for debugging/NATS interaction | `false`                           |
| `nats.podTemplate.patch`               | JSON Patch applied to NATS pods for initializing sysctl parameters | `net.core.somaxconn: 16384`          |
| `nats.config.cluster.noAdvertise`      | Prevent clients from discovering internal routes of the NATS cluster | `true`                               |
| `nats.config.cluster.enabled`          | Enables the formation of a NATS cluster                        | `true`                                |
| `nats.config.cluster.replicas`         | Number of NATS server replicas in the cluster                  | `3`                                   |
| `nats.config.cluster.merge.pool_size`  | NATS server pool size to fine-tune performance                 | `1024`                                |
| `nats.config.gateway.enabled`          | Enables cross-cluster communication gateways                   | `false`                               |
| `nats.jetstream.enabled`               | Enables Jetstream, NATS' persistence storage system            | `true`                                |
| `nats.jetstream.memoryStore.enabled`   | Enables in-memory storage for Jetstream                        | `true`                                |
| `nats.jetstream.memoryStore.maxSize`   | Maximum size for the in-memory storage for Jetstream           | `2Gi`                                 |
| `nats.jetstream.fileStore.enabled`     | Enables file-based storage for Jetstream                       | `true`                                |
| `nats.jetstream.fileStore.storageDirectory` | Directory for storing Jetstream data on file storage          | `/data`                               |
| `nats.jetstream.fileStore.pvc.enabled` | Enables PersistentVolumeClaim for storage persistence          | `true`                                |
| `nats.jetstream.fileStore.pvc.size`    | Size of the PersistentVolumeClaim for Jetstream file storage   | `2Gi`                                 |
| `nats.jetstream.fileStore.pvc.storageClassName` | Storage class for high I/O performance                       | `block-storage-high-speed`            |
| `nats.container.merge.resources.requests` | Resources requests for NATS containers (cpu, memory, ephemeral-storage) | `cpu: 1, memory: 1Gi, ephemeral-storage: 1Gi` |
| `nats.container.merge.resources.limits` | Resources limits for NATS containers (cpu, memory, ephemeral-storage) | `cpu: 3, memory: 3Gi, ephemeral-storage: 1Gi` |
| `promExporter.enabled`                 | Enables a sidecar for Prometheus metrics exporting              | `true`                                |
| `promExporter.podMonitor.enabled`      | Enables Prometheus monitoring of NATS pod metrics               | `true`                                |
| `promExporter.merge.resources.requests` | Resources requests for Prometheus exporter container (cpu, memory) | `cpu: 1, memory: 500Mi`              |
| `promExporter.merge.resources.limits`  | Resources limits for Prometheus exporter container (cpu, memory) | `cpu: 1, memory: 500Mi`              |
| `podDisruptionBudget.enabled`          | Enables Pod Disruption Budget to maintain high availability     | `false`                               |
| `podDisruptionBudget.maxUnavailable`   | Maximum number or percentage of unavailable pods during disruption | `1`                                 |

## Monitoring

### Grafana Dashboard

You can find a rich sample of the Grafana dashboard example useful for visualizing NATS metrics in the [monitoring](./monitoring/grafana-dashboard.json) folder. You should customize it according to your requirements.

### Alerting

You can find useful alerts to set on the NATS cluster in the [monitoring](./monitoring/prometheus-rule.yaml) folder. You should customize it according to your requirements.
