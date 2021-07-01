# OpenShift CICD Demo

This chart deploys the [OpenShift CICD Demo](https://github.com/RedHatWorkshops/openshift-cicd-demo) on your cluster.

# Introduction

This chart deploys a Job that applies the needed manifests that demos
OpenShift's CICD capabilites. The demo includes an "all-in-one", self
contained, demo of using Tekton as your CI system while using Argo CD
for the CD portion.

The demo also includes Gitea to use as
your SCM for the demo. Please see the [demo repository](https://github.com/RedHatWorkshops/openshift-cicd-demo)
for more information.

# Prerequisites

* Helm v3
* OpenShift 4.7 or newer
* Cluster Administrator access

# Installing the Chart

Users installing this chart must have Cluster Administrator permissions.

To install the chart with the release name `my-release`:

```shell
helm
```

# Post Installation

See the instructions (from NOTES.txt within chart) after the helm
installation completes for information about the demo . The instruction can also
be viewed by running the command: `helm status my-release`


# Configuration

The following table lists the configurable parameters and the default values.

| Parameter | Description | Default |
| ----------| ----------- | ------- |
| `timeoutoptions.sleep`  | How long to sleep between failures   | `5` |
| `timeoutoptions.threshold` | Maximum itterations to try before giving up | `60` |

# Limitations

* This chart can only be installed once per cluster.
* User MUST have cluster admin access
* Uninstall will ALSO uninstall OpenShift GitOps
* This chart assumes an "empty" OpenShift cluster
