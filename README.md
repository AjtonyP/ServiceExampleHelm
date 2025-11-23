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

## Automated Chart Updates

This repository includes automated workflows for publishing and updating the Helm chart.

### Chart Release Workflow

When `Chart.yaml` is updated on `main` branch:

1. Chart is packaged with `helm package`
2. GitHub release is created with the chart archive
3. Chart index is updated and published to GitHub Pages
4. Chart becomes available at `https://ajtonyp.github.io/ServiceExampleHelm`

### Chart Update Workflow

Triggered automatically by ServiceExampleCI when a new Docker image is built:

1. Receives version number via `repository_dispatch` event
2. Updates `Chart.yaml` version and appVersion
3. Updates `values.yaml` default image tag
4. Commits and pushes changes
5. Triggers the release workflow above

### Chart Structure

```
serviceexample/
├── Chart.yaml              # Chart metadata
├── values.yaml            # Default configuration
├── values.schema.json     # Configuration validation
└── templates/
    ├── app-deployment.yaml    # ServiceExample deployment
    ├── app-service.yaml       # Application service
    ├── mongodb.yaml           # MongoDB replica set
    ├── redis.yaml             # Redis deployment
    └── nats.yaml              # NATS deployment
```

## Artifact Hub

This chart is published to [Artifact Hub](https://artifacthub.io/)

Metadata includes:

- Container images used
- Maintainer information
- Changelog tracking
- License information
