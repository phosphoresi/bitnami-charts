# Tomcat

[Apache Tomcat](http://tomcat.apache.org/), often referred to as Tomcat, is an open-source web server and servlet container developed by the Apache Software Foundation. Tomcat implements several Java EE specifications including Java Servlet, JavaServer Pages, Java EL, and WebSocket, and provides a "pure Java" HTTP web server environment for Java code to run in.

## TL;DR;

```console
$ helm repo add bitnami https://charts.bitnami.com/bitnami
$ helm install bitnami/tomcat
```

## Introduction

This chart bootstraps a [Tomcat](https://github.com/bitnami/bitnami-docker-tomcat) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

Bitnami charts can be used with [Kubeapps](https://kubeapps.com/) for deployment and management of Helm Charts in clusters. This Helm chart has been tested on top of [Bitnami Kubernetes Production Runtime](https://kubeprod.io/) (BKPR). Deploy BKPR to get automated TLS certificates, logging and monitoring for your applications.

## Prerequisites

- Kubernetes 1.4+ with Beta APIs enabled
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm repo add bitnami https://charts.bitnami.com/bitnami
$ helm install --name my-release bitnami/tomcat
```

These commands deploy Tomcat on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following tables lists the configurable parameters of the Tomcat chart and their default values.

| Parameter                            | Description                                                                                                                                               | Default                                                 |
| ------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| `global.imageRegistry`               | Global Docker image registry                                                                                                                              | `nil`                                                   |
| `global.imagePullSecrets`            | Global Docker registry secret names as an array                                                                                                           | `[]` (does not add image pull secrets to deployed pods) |
| `global.storageClass`                     | Global storage class for dynamic provisioning                                               | `nil`                                                        |
| `image.registry`                     | Tomcat image registry                                                                                                                                     | `docker.io`                                             |
| `image.repository`                   | Tomcat Image name                                                                                                                                         | `bitnami/tomcat`                                        |
| `image.tag`                          | Tomcat Image tag                                                                                                                                          | `{TAG_NAME}`                                            |
| `image.pullPolicy`                   | Tomcat image pull policy                                                                                                                                  | `IfNotPresent`                                          |
| `image.pullSecrets`                  | Specify docker-registry secret names as an array                                                                                                          | `[]` (does not add image pull secrets to deployed pods) |
| `nameOverride`                       | String to partially override tomcat.fullname template with a string (will prepend the release name)                                                       | `nil`                                                   |
| `fullnameOverride`                   | String to fully override tomcat.fullname template with a string                                                                                           | `nil`                                                   |
| `volumePermissions.enabled`          | Enable init container that changes volume permissions in the data directory (for cases where the default k8s `runAsUser` and `fsUser` values do not work) | `false`                                                 |
| `volumePermissions.image.registry`   | Init container volume-permissions image registry                                                                                                          | `docker.io`                                             |
| `volumePermissions.image.repository` | Init container volume-permissions image name                                                                                                              | `bitnami/minideb`                                       |
| `volumePermissions.image.tag`        | Init container volume-permissions image tag                                                                                                               | `latest`                                                |
| `volumePermissions.image.pullPolicy` | Init container volume-permissions image pull policy                                                                                                       | `Always`                                                |
| `volumePermissions.resources`        | Init container resource requests/limit                                                                                                                    | `nil`                                                   |
| `tomcatUsername`                     | Tomcat admin user                                                                                                                                         | `user`                                                  |
| `tomcatPassword`                     | Tomcat admin password                                                                                                                                     | _random 10 character alphanumeric string_               |
| `tomcatAllowRemoteManagement`        | Enable remote access to management interface                                                                                                              | `0` (disabled)                                          |
| `securityContext.enabled`            | Enable security context                                                                                                                                   | `true`                                                  |
| `securityContext.fsGroup`            | Group ID for the container                                                                                                                                | `1001`                                                  |
| `securityContext.runAsUser`          | User ID for the container                                                                                                                                 | `1001`                                                  |
| `service.type`                       | Kubernetes Service type                                                                                                                                   | `LoadBalancer`                                          |
| `service.port`                       | Service HTTP port                                                                                                                                         | `80`                                                    |
| `service.nodePorts.http`             | Kubernetes http node port                                                                                                                                 | `""`                                                    |
| `service.externalTrafficPolicy`      | Enable client source IP preservation                                                                                                                      | `Cluster`                                               |
| `service.loadBalancerIP`             | LoadBalancer service IP address                                                                                                                           | `""`                                                    |
| `persistence.enabled`                | Enable persistence using PVC                                                                                                                              | `true`                                                  |
| `persistence.storageClass`           | PVC Storage Class for Tomcat volume                                                                                                                       | `nil` (uses alpha storage class annotation)             |
| `persistence.accessMode`             | PVC Access Mode for Tomcat volume                                                                                                                         | `ReadWriteOnce`                                         |
| `persistence.size`                   | PVC Storage Request for Tomcat volume                                                                                                                     | `8Gi`                                                   |
| `resources`                          | CPU/Memory resource requests/limits                                                                                                                       | Memory: `512Mi`, CPU: `300m`                            |
| `ingress.enabled` | Enable the ingress controller | `false` |
| `ingress.certManager` | Add annotations for certManager | `false` |
| `ingress.annotations` | Annotations to set in the ingress controller | - |
| `ingress.hosts` | List of hostnames to be covered with the ingress | `tomcat.local` |
| `ingress.tls` | List with TLS configuration for the ingress | `hosts: tomcat.local, secretName: tomcat.local-tls` | 

The above parameters map to the env variables defined in [bitnami/tomcat](http://github.com/bitnami/bitnami-docker-tomcat). For more information please refer to the [bitnami/tomcat](http://github.com/bitnami/bitnami-docker-tomcat) image documentation.

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
$ helm install --name my-release \
  --set tomcatUser=manager,tomcatPassword=password bitnami/tomcat
```

The above command sets the Tomcat management username and password to `manager` and `password` respectively.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```console
$ helm install --name my-release -f values.yaml bitnami/tomcat
```

> **Tip**: You can use the default [values.yaml](values.yaml)

### [Rolling VS Immutable tags](https://docs.bitnami.com/containers/how-to/understand-rolling-tags-containers/)

It is strongly recommended to use immutable tags in a production environment. This ensures your deployment does not change automatically if the same tag is updated with a different image.

Bitnami will release a new chart updating its containers if a new version of the main container, significant changes, or critical vulnerabilities exist.

## Persistence

The [Bitnami Tomcat](https://github.com/bitnami/bitnami-docker-tomcat) image stores the Tomcat data and configurations at the `/bitnami/tomcat` path of the container.

Persistent Volume Claims are used to keep the data across deployments. This is known to work in GCE, AWS, and minikube.
See the [Configuration](#configuration) section to configure the PVC or to disable persistence.

### Adjust permissions of persistent volume mountpoint

As the image run as non-root by default, it is necessary to adjust the ownership of the persistent volume so that the container can write data into it.

By default, the chart is configured to use Kubernetes Security Context to automatically change the ownership of the volume. However, this feature does not work in all Kubernetes distributions.
As an alternative, this chart supports using an initContainer to change the ownership of the volume before mounting it in the final destination.

You can enable this initContainer by setting `volumePermissions.enabled` to `true`.

## Upgrading

### To 2.1.0

Tomcat container was moved to a non-root approach. There shouldn't be any issue when upgrading since the corresponding `securityContext` is enabled by default. Both the container image and the chart can be upgraded by running the command below:

```
$ helm upgrade my-release stable/tomcat
```

If you use a previous container image (previous to **8.5.35-r26**) disable the `securityContext` by running the command below:

```
$ helm upgrade my-release stable/tomcat --set securityContext.enabled=fase,image.tag=XXX
```

### To 1.0.0

Backwards compatibility is not guaranteed unless you modify the labels used on the chart's deployments.
Use the workaround below to upgrade from versions previous to 1.0.0. The following example assumes that the release name is tomcat:

```console
$ kubectl patch deployment tomcat --type=json -p='[{"op": "remove", "path": "/spec/selector/matchLabels/chart"}]'
```
