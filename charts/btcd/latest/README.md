# btcd

[Bitcoin](https://btcinformation.org/) uses peer-to-peer technology to operate with no central authority or banks;
managing transactions and the issuing of bitcoins is carried out collectively by the network.

## Introduction

This chart bootstraps a single node btcd deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.
Docker image was based on [docker-bitcoind](https://github.com/kylemanna/docker-bitcoind) - many thanks!

## Prerequisites

- Kubernetes 1.8+
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install --name my-release stable/btcd
```

The command deploys btcd on the Kubernetes cluster in the default configuration.
The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the btcd chart and their default values.

| Parameter                | Description                         | Default         |
| ------------------------ | ----------------------------------- | --------------- |
| `image.repository`       | Image source repository name        | `lnzap/btcd`    |
| `image.tag`              | `btcd` release tag.                 | `latest`        |
| `image.pullPolicy`       | Image pull policy                   | `IfNotPresent`  |
| `service.rpcPort`        | RPC port                            | `8334`          |
| `service.p2pPort`        | P2P port                            | `8333`          |
| `service.testnetPort`    | Testnet port                        | `18334`         |
| `service.testnetP2pPort` | Testnet p2p ports                   | `18333`         |
| `persistence.enabled`    | Create a volume to store data       | `true`          |
| `persistence.accessMode` | ReadWriteOnce or ReadOnly           | `ReadWriteOnce` |
| `persistence.size`       | Size of persistent volume claim     | `300Gi`         |
| `resources`              | CPU/Memory resource requests/limits | `{}`            |
| `configurationFile`      | Config file ConfigMap entry         |

For more information about btcd configuration please see [btcd.conf configuration file](https://github.com/btcsuite/btcd/tree/master/docs#Configuration).

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install --name my-release -f values.yaml stable/btcd
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Persistence

The btcd image stores the btcd node data (Blockchain and wallet) and configurations at the `/btcd` path of the container.

By default a PersistentVolumeClaim is created and mounted into that directory. In order to disable this functionality
you can change the values.yaml to disable persistence and use an emptyDir instead.

> _"An emptyDir volume is first created when a Pod is assigned to a Node, and exists as long as that Pod is running on that node. When a Pod is removed from a node for any reason, the data in the emptyDir is deleted forever."_

!!! WARNING !!!

Please NOT use emptyDir for production cluster! Your wallets will be lost on container restart!

## Customize btcd configuration file

```yaml
configurationFile:
  btcd.conf: |-
    rpcuser=rpcuser
    rpcpassword=rpcpassword
```
