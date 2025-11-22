# ServiceExampleHelm

Helm chart for ServiceExample .NET application and its dependencies

## Installation

Add the Helm repository:

```bash
helm repo add serviceexample https://ajtonyp.github.io/ServiceExampleHelm
helm repo update
```

Install the chart:

```bash
helm install my-serviceexample serviceexample/serviceexample
```

Or install from the local repository:

```bash
helm install my-serviceexample .
```

## Configuration

See `values.yaml` for configuration options. The chart includes:

- **ServiceExample Application**: Main .NET application (default image: `ajtonyp/serviceexample:0.0.1`)
- **MongoDB**: 3-member replica set with Longhorn persistent storage
- **Redis**: Cache with persistent storage and metrics exporter
- **NATS**: Messaging system with monitoring endpoints

### Key Configuration Options

```yaml
app:
  image:
    repository: ajtonyp/serviceexample
    tag: "0.0.1"
  service:
    type: NodePort
    nodePort: 30080

mongodb:
  replicas: 3
  persistence:
    storageClass: longhorn
    size: 2Gi

redis:
  persistence:
    storageClass: longhorn
    size: 1Gi

global:
  storageClass: longhorn
```

## Requirements

- Kubernetes 1.19+
- Helm 3+
- Longhorn storage class

## Observability

All components are configured with:

- Prometheus scrape annotations
- Health/liveness/readiness probes
- Metrics endpoints
