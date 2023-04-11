# K8s Microservice Template &middot; [![GitHub Release](https://img.shields.io/github/v/release/pagopa/eks-microservice-chart-blueprint?style=flat)](https://github.com/pagopa/eks-microservice-chart-blueprint/releases) [![GitHub Issues](https://img.shields.io/github/issues/pagopa/eks-microservice-chart-blueprint?style=flat)](https://github.com/pagopa/eks-microservice-chart-blueprint/issues) [![Open Source](https://badges.frapsoft.com/os/v1/open-source.svg?v=103)](https://opensource.org/)

The `eks-microservice-chart-blueprint` chart is the best way to release your
microservice into PagoPA K8s environment. It contains all the required
components to get started, and it has several architectural aspects already
configured.

Some of the key benefits of this chart are:

- Highly secure environment thanks to Secret Store CSI Provider;
- Ingress HTTPS connection;
- Improved scalability and reliability thanks to **Keda**;
- Simpified way to setup secrets and configMaps

## Architecture

To see the entire architecture please see this page [architecture](docs/ARCHITECTURE.md)

## Changelog

see [CHANGELOG](CHANGELOG) to see the new features and the breking changes

## Pre requisites

- helm & kubernetes

### Static analysis

Install:

- <https://github.com/norwoodj/helm-docs>

## Installation

This is the official and recommended method to adopt this chart.

### Quick start

Create a `helm` folder inside your microservice project in which install the
Helm chart:

```shell
mkdir helm && cd helm
```

Add Helm repo:

```shell
helm repo add pagopa-microservice https://pagopa.github.io/eks-microservice-chart-blueprint
```

> If you had already added this repo earlier, run `helm repo update` to retrieve
> the latest versions of the packages.

Add a very basic configuration in `Chart.yaml`:

```shell
cat <<EOF > Chart.yaml
apiVersion: v2
name: my-microservice
description: My microservice description
type: application
version: 1.0.0
appVersion: 1.0.0
dependencies:
- name: microservice-chart
  version: 1.19.0
  repository: "https://pagopa.github.io/eks-microservice-chart-blueprint"
EOF
```

Install the dependency:

```shell
helm dep build
```

Create a `values-<env>.yaml` for each environment:

```shell
touch values-dev.yaml values-uat.yaml values-prod.yaml
```

Override all values that you need, and form the root of your project install
the chart:

```sh
helm upgrade -i -n <namespace name> -f <file with values> <name of the helm chart> <chart folder>

helm upgrade -i -n mynamespace -f helm/values-dev.yaml mymicroservice helm
```

### Upgrading

Change version of the dependency and run the update:

```shell
cd helm && helm dep update .
```

## Template/App mandatory resources and configuration

To work as expect this template must request:

App:

- has liveness and readiness endpoints
- you know which are the probes for your application, because are mandatory

Azure:

- TLS certificate are present into the kv (for ingress)
- Managed POD identity are created

K8s:

- Reloader of other tools that allow to restar the pod in case of some of the config map or secret are changed

## Final Result

Here you can find a result of the template [final result](docs/FINAL_RESULT_EXAMPLE.md)

## Examples

In the [`example`](example/) folder, you can find a working examples.

### Example #1

## Advanced

For more information, visit the [complete documentation](https://pagopa.atlassian.net/wiki/spaces/DEVOPS/pages/479658690/Microservice+template).

## Development

Clone the repository and run the setup script:

```shell
git clone git@github.com:pagopa/eks-microservice-chart-blueprint.git
cd eks-microservice-chart-blueprint.git
sh /bin/setup
```

### Warning

Setup script installs a version manager tool that may introduce
compatibility issues in your environment. To prevent any potential
problems, you can install these dependencies manually or with your
favourite tool:

- NodeJS 14.17.3
- Helm 3.8.0

### Publish

The branch `gh-pages` contains the GitHub page content and all released charts.
To update the page content, use `bin/publish`.

## Known issues and limitations

- None.
