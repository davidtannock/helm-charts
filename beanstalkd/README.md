# Beanstalkd

> [Beanstalkd](http://kr.github.io/beanstalkd/) is a simple, fast work queue. Its interface is generic, but was originally designed for reducing the latency of page views in high-volume web applications by running time-consuming tasks asynchronously.

Influenced by the official [Memcached](https://github.com/kubernetes/charts/tree/master/stable/memcached) chart.

## TL;DR;

```bash
$ helm repo add dtannock-charts https://davidtannock.github.io/helm-charts/
$ helm install dtannock-charts/beanstalkd
```
## Introduction

This chart bootstraps a Beanstalkd deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

The Beanstalkd binary log (for fault tolerance) is enabled and bound to a persistent volume.

A [StatefulSet](https://kubernetes.io/docs/tutorials/stateful-application/basic-stateful-set/) is used to support multiple Beanstalkd instances. DNS is the recommended method of service discovery.

## Installing the Chart

To add the Helm repository with the name `dtannock-charts`:

```bash
$ helm repo add dtannock-charts https://davidtannock.github.io/helm-charts/
```

To install the chart with the release name `my-release`:

```bash
$ helm install --name my-release dtannock-charts/beanstalkd
```

The command deploys Beanstalkd on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following tables lists the configurable parameters of the Beanstalkd chart and their default values.

|        Parameter          |                      Description                       |                Default               |
|---------------------------|--------------------------------------------------------|--------------------------------------|
| `image`                   | The image to pull and run                              | `dtannock/beanstalkd:1.10`           |
| `imagePullPolicy`         | Image pull policy                                      | `IfNotPresent`                       |
| `beanstalkd.maxJobSize`   | The maximum size of a job in bytes                     | `65535`                              |
| `beanstalkd.binlogSize`   | The size of the binary log volume                      | `20Mi`                               |
| `service.port`            | The port the headless service should listen on         | `11300`                              |
| `metrics.enabled`         | Start a side-car prometheus exporter                   | false                                |
| `metrics.image`           | Beanstalkd metrics exporter image                      | `dtannock/beanstalkd-exporter:0.1.1` |
| `metrics.imagePullPolicy` | Metrics image pull policy                              | `IfNotPresent`                       |
| `metrics`.`port`          | The port the headless metrics service should listen on | `11301`                              |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```bash
$ helm install --name my-release \
  dtannock-charts/beanstalkd \
  --set beanstalkd.maxJobSize=131070
```

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install --name my-release -f values.yaml dtannock-charts/beanstalkd
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Minikube

At this time, [Minikube](https://github.com/kubernetes/minikube) only supports a single node Kubernetes cluster.

If you plan on running more than one replica, you must change the node affinity like the following:

```bash
$ helm install --name my-release \
  dtannock-charts/beanstalkd \
  --set AntiAffinity=soft \
  --set replicas=2
```

