# kvrock-standalone Helm Chart

Helm chart for running [Apache Kvrocks](https://github.com/apache/kvrocks) on Kubernetes with three topology modes:

- `standalone`: single StatefulSet + Service.
- `replication`: master + replicas.
- `sentinel`: master + replicas + Redis Sentinel.

## Prerequisites

- Kubernetes 1.24+
- Helm 3.8+
- A default StorageClass (or set `persistence.storageClassName`)

## Install

```bash
helm install kvrocks ./kvrock-standalone
```

Install with custom values:

```bash
helm install kvrocks ./kvrock-standalone -f my-values.yaml
```

## Upgrade

```bash
helm upgrade kvrocks ./kvrock-standalone -f my-values.yaml
```

## Uninstall

```bash
helm uninstall kvrocks
```

## Modes

Set `mode` in `values.yaml`:

- `standalone`: basic Kvrocks deployment.
- `replication`: adds replica StatefulSet and replica Service.
- `sentinel`: adds Redis Sentinel StatefulSet and Service for failover orchestration.

## Configuration

Full config reference and examples are documented in:

- [`docs/configuration.md`](docs/configuration.md)

## Notes

- `service.port` is the Service port exposed for Kvrocks.
- `kvrocks.port` is the container/listening port used in `kvrocks.conf`.
- `persistence.enabled` exists in `values.yaml`, but current templates always create PVCs and do not branch on this flag.
