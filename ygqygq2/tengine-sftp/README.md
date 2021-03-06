# tengine-sftp - 有sftp功能的nginx

[tengine-sftp](https://github.com/ygqygq2/charts/tree/master/tengine-sftp)是在tengine基础上添加了sftp功能，sftp可用于管理web文件。

## Introduction

This chart bootstraps tengine-sftp deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.6+
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install my-release tengine-sftp
```

The command deploys tengine-sftp cluster on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.
>tip:
>The default sftp user is: dev
>The default sftp password is: dev

### Uninstall

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release
```

## Configuration

The following table lists the configurable parameters of the tengine-sftp chart and their default values.

| Parameter                  | Description                         | Default                                |
| -----------------------    | ----------------------------------- | -------------------------------------- |
| `statefulset.enabled`      | use statefulset to start            | `false`                                |
| `deploymentStrategy`       | deployment rollingUpdate setting    | `{}`                                   |
| `replicaCount`             | replicas number                     | `1`                                    |
| `service`                  | Service type, protocol, port        | `ClusterIP` `TCP` 8080, 22             |
| `env`                      | container env setting               | `[]`                                   |
| `startCommand`             | Start command                       | `[]`                                   |
| `config`                   | Additional configmap to use         | see in `values.yaml`                   |
| `secret`                   | Additional secret to use            | see in `values.yaml`                   |
| `image`                    | nginx image, tag.                   | `ygqygq2/tengine` `2.3.2`              |
| `ingress`                  | Ingress for the tengine-sftp.       | `false`                                |
| `metrics.enabled`          | Prometheus metrics                  | `false`                                |
| `persistentVolume.enabled` | Create a volume to store data       | `false`                                |
| `persistentVolume.storageClass` | Type of persistent volume claim| `nil`                                  |
| `persistentVolume.accessModes`  | Persistent volume access modes | `[ReadWriteMany]`                      |
| `persistentVolume.size`         | Persistent volume access modes | `500Mi`                                |
| `persistentVolume.existingClaim`| Persistent volume existingClaim name| `{}`                              |
| `persistentVolume.mountPaths`   | Persistent directory path      | see in `values.yaml`                   |
| `persistentVolume.annotations`  | Persistent volume annotations  | `{}`                                   |
| `healthCheck.enabled`      | Liveness and readiness probes       | `false`, detail see in `values.yaml`    |
| `resources`                | CPU/Memory resource requests/limits | `{}`                                   |
| `lifecycle`                | Pod lifecycle                       | `{}`                                   |
| `deployment.additionalVolumes`| Deployment additionalVolumes     | `[]`                                   |
| `additionalContainers`     | Sidecar containers                  | `{}`                                   |
| `sftp`                     | sftp to upload files                | see more in `values.yaml`              |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`.

## Persistence

The two cantainers share the volume, then sftp can manage nginx's files.
The [sftp image](https://github.com/ygqygq2/sftp) stores the data and configurations at the `/home/dev/upload` path of the container.

