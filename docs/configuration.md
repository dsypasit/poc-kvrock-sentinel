# Configuration

This chart is configured through `values.yaml`.

## Topology Mode

| Key | Type | Default | Description |
|---|---|---|---|
| `mode` | string | `sentinel` | Deployment mode: `standalone`, `replication`, or `sentinel`. |

## Global Naming

| Key | Type | Default | Description |
|---|---|---|---|
| `nameOverride` | string | `""` | Override chart name part used in generated resource names. |
| `fullnameOverride` | string | `""` | Fully override generated resource base name. |

## Standalone Replica Count

| Key | Type | Default | Description |
|---|---|---|---|
| `replicaCount` | int | `1` | Replica count used by the standalone StatefulSet template. |

## Kvrocks Image

| Key | Type | Default | Description |
|---|---|---|---|
| `image.repository` | string | `apache/kvrocks` | Kvrocks image repository. |
| `image.tag` | string | `2.14.0` | Kvrocks image tag. |
| `image.pullPolicy` | string | `IfNotPresent` | Image pull policy for Kvrocks pods. |

## Sentinel Image

| Key | Type | Default | Description |
|---|---|---|---|
| `sentinelImage.repository` | string | `redis` | Sentinel image repository (Redis image). |
| `sentinelImage.tag` | string | `7.2-alpine` | Sentinel image tag. |
| `sentinelImage.pullPolicy` | string | `IfNotPresent` | Image pull policy for Sentinel pods. |

## Service

| Key | Type | Default | Description |
|---|---|---|---|
| `service.type` | string | `ClusterIP` | Kubernetes Service type. |
| `service.port` | int | `6666` | Service port for Kvrocks Services (master/replica/standalone). |

## Kvrocks Runtime

| Key | Type | Default | Description |
|---|---|---|---|
| `kvrocks.bind` | string | `0.0.0.0` | `bind` directive in `kvrocks.conf`. |
| `kvrocks.port` | int | `6666` | `port` directive in `kvrocks.conf` and container port. |
| `kvrocks.dir` | string | `/var/lib/kvrocks` | Data directory path in container and `kvrocks.conf`. |
| `kvrocks.logLevel` | string | `info` | `log-level` directive in `kvrocks.conf`. |

## Persistence

| Key | Type | Default | Description |
|---|---|---|---|
| `persistence.enabled` | bool | `true` | Declared value, but currently not used in template conditionals. |
| `persistence.size` | string | `5Gi` | PVC request size for Kvrocks StatefulSets. |
| `persistence.storageClassName` | string | `""` | StorageClass for PVCs. Empty means cluster default. |

## Resources

| Key | Type | Default | Description |
|---|---|---|---|
| `resources.requests.cpu` | string | `100m` | CPU request for standalone StatefulSet container template. |
| `resources.requests.memory` | string | `256Mi` | Memory request for standalone StatefulSet container template. |
| `resources.limits.cpu` | string | `500m` | CPU limit for standalone StatefulSet container template. |
| `resources.limits.memory` | string | `512Mi` | Memory limit for standalone StatefulSet container template. |

## Replication

| Key | Type | Default | Description |
|---|---|---|---|
| `replication.replicaCount` | int | `2` | Number of Kvrocks replica pods in replication/sentinel modes. |

## Sentinel

| Key | Type | Default | Description |
|---|---|---|---|
| `sentinel.replicaCount` | int | `3` | Number of Sentinel pods. |
| `sentinel.port` | int | `26379` | Sentinel TCP port. |
| `sentinel.masterName` | string | `mymaster` | Master name used in Sentinel config directives. |
| `sentinel.quorum` | int | `2` | Quorum for Sentinel failover voting. |
| `sentinel.downAfterMilliseconds` | int | `5000` | Timeout before master is considered down. |
| `sentinel.failoverTimeout` | int | `10000` | Timeout for failover operations. |
| `sentinel.parallelSyncs` | int | `1` | Number of replicas to re-sync in parallel during failover. |

## Example Overrides

### Standalone mode

```yaml
mode: standalone
replicaCount: 1
service:
  type: ClusterIP
  port: 6666
```

### Replication mode

```yaml
mode: replication
replication:
  replicaCount: 2
```

### Sentinel mode

```yaml
mode: sentinel
replication:
  replicaCount: 2
sentinel:
  replicaCount: 3
  quorum: 2
  masterName: mymaster
```
