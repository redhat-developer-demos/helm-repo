# Netcandy Store

This chart deploys the [Netcandy Store](https://github.com/redhat-developer-demos/netcandystore) on an
OpenShift cluster running Windows Containers.

# Introduction

This chart deploys the Netcandy store. This is a sample application that
runs with Linux containers and Windows containers working together to
deliver an application.

High level diagram:

![multi-platform](http://people.redhat.com/chernand/windows-containers-quickstart/images/mixed-windows-and-linux-workloads.png)

Once deployed you will have an application that is using both Linux and Windows containers.

![netcandy store](http://people.redhat.com/chernand/windows-containers-quickstart/images/ncs.png)

# Prerequisites

* Helm v3
* OpenShift 4.6 or newer
* OVN with Hybridoverlay networking configured
* Windows Node installed
* Cluster Administrator access

# Pre-Install Steps

Add the repository.

```shell
helm repo add redhat-demos https://redhat-developer-demos.github.io/helm-repo
```

It's good to update all the definitions as well.

```shell
helm repo update
```

Make sure you have a Windows node running and ready. 

```shell
$ oc get nodes -l kubernetes.io/os=windows
NAME                                        STATUS   ROLES    AGE   VERSION
ip-10-0-147-19.us-east-2.compute.internal   Ready    worker   46h   v1.19.0-rc.2.1023+f5121a6a6a02dd
```

Please view the [offical OpenShift docs](https://docs.openshift.com/container-platform/4.7/windows_containers/windows-containers-release-notes-2-x.html) for more information about installing Windows nodes.

# Installing the Chart

Users installing this chart must have Cluster Administrator permissions.

First create and label the namespace used by the help chart:

```shell
oc create namespace netcandystore
oc label --overwrite namespace netcandystore \
pod-security.kubernetes.io/enforce=privileged \
pod-security.kubernetes.io/warn=baseline \
pod-security.kubernetes.io/audit=baseline \
security.openshift.io/scc.podSecurityLabelSync=false
```

The labels are required as the application runs with `ContainerAdministrator`
user and that requires elevated privileges. You can now install the chart with
the release name `ncs`:

```shell
helm install ncs --namespace netcandystore \
--timeout=1200s \
redhat-demos/netcandystore
```

# Uninstalling the Chart

To uninstall/delete the (for example `my-release`) deployment:

```shell
helm uninstall ncs -n netcandystore
```

# Configuration

The following table lists the configurable parameters and the default values.

| Parameter | Description | Default |
| ----------| ----------- | ------- |
| `netcandy.image` | Image to use for the Netcandy store backend .NET application | `quay.io/donschenck/netcandystore:2021mar8.1` |
| `netcandy.dbpassword` | Password to set for the database | `reallylongpassword99!` |
| `netcandy.dbstorage` | Storage size for the database | `1Gi` |
| `netcandy.dbimage` | Image to use for the MSSQL Database | `mcr.microsoft.com/mssql/rhel/server:2019-latest` |
| `frontend.replicas` | The amount of frontend replicas to run | `1` |
| `backend.replicas` | The amount of backend replicas to run | `1` |
| `route.enabled` | Create a route for the application | `true` |

Please note, the default image `quay.io/donschenck/netcandystore:2021mar8.1` is specific to
Windows Server 2019 LTSC 1809. It may not run if you're not
running that specific version of Windows. For example if you're
running Windows Containers on VMWare you'll most likely need to use
`quay.io/gfontana/netcandystore:servercore1909` as your `netcandy.image`
setting.

It's also possible to compile your own version of [Netcandy Store](https://github.com/redhat-developer-demos/netcandystore) and use that as your `netcandy.image`. YMMV.

Consult the [offical OpenShift docs](https://docs.openshift.com/container-platform/4.7/windows_containers/windows-containers-release-notes-2-x.html) for more information about installing Windows container versions.

# Limitations

* Cluster admin access is **REQUIRED**
* Windows Node **MUST** be installed/running
