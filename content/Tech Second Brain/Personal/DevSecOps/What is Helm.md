---
title: What is Helm ?
tags:
  - devops
  - k8s
  - helpful
---
# Installation

![[Pasted image 20240404100820.png]]

*Official Homepage: https://helm.sh/*

>[!info]
><h2>What is Helm ? </h2>
>Helm helps you manage Kubernetes applications — Helm Charts help you define, install, and upgrade even the most complex Kubernetes application.
>
>Charts are easy to create, version, share, and publish — so start using Helm and stop the copy-and-paste.
>
>Helm is a graduated project in the CNCF and is maintained by the Helm community.

Helm already installing and using on CI/CD, If you want to learn and work with `Helm`, feel free to installing on [Installing Guide](https://helm.sh/docs/intro/install/#helm)

# Helm Template

All of application will base on template for releasing, you can understanding the concept with [helm template](https://helm.sh/docs/chart_best_practices/templates/)

```bash
mychart
|-- Chart.yaml
|-- charts
|-- templates
|   |-- NOTES.txt
|   |-- _helpers.tpl
|   |-- deployment.yaml
|   |-- ingress.yaml
|   `-- service.yaml
`-- values.yaml
```

> [!question]
> <h2>Why we need the helm template ?</h2>
> 
> - Make everything become simple for managing and replacing
> - Changing only one place and anything will come update for each environment (**Consider when you want to change, make sure anything is working**)
> - Flexible variables base on environment with `values.yaml` file

*Related articles:*

- [Understand A Helm Chart Structure](https://docs.bitnami.com/kubernetes/faq/administration/understand-helm-chart/)
- [The Chart Template Developer's Guide](https://helm.sh/docs/chart_template_guide/)

# Priority of Value in Helm

![[Pasted image 20240404140117.png]]

>[!important]
>To override values in a chart
>- use either the `--values` flag and pass in a file or use the `--set` flag and pass configuration from the command line
>- To force string values, use `--set-string`. 
>- You can use `--set-file` to set individual values from a file when the value itself is too long for the command line or is dynamically generated. 
>- You can also use `--set-json` to set json values `(scalars/objects/arrays)` from the command line.
>
>You can specify the `--values'/'-f` flag multiple times. The priority will be given to the **last (right-most)** file specified.
>
>You can specify the `--set` flag multiple times. The priority will be given to the **last (right-most)** set specified.
>
>You can update the values for an existing release with this command as well via the `--reuse-values` flag. The `RELEASE` and `CHART` arguments should be **set to the original parameters**, and existing values will be merged with any values set via `--values'/'-f` or `--set` flags.

*Related articles*

- [Understanding Helm upgrade flags](https://shipmight.com/blog/understanding-helm-upgrade-reset-reuse-values)
# Command usage

`Helm` will submit the role for both of CI and CD progress*

![[Pasted image 20240404143727.png]]
## CI (Continuous Integration)

<h2>helm dependency</h2>

*Documentation: [doc](https://helm.sh/docs/helm/helm_dependency/#helm)*

**Purpose: manage a chart's dependencies**

>[!info]
>Manage the dependencies of a chart.
>
>Helm charts store their dependencies in 'charts/'. For chart developers, it is often easier to manage dependencies in 'Chart.yaml' which declares all dependencies.
>
>The dependency commands operate on that file, making it easy to synchronize between the desired dependencies and the actual dependencies stored in the 'charts/' directory.

Just use `update` optional for *update charts/ based on the contents of Chart.yaml*

Example:

*Documentation: [The ==Chart.yaml== File](https://helm.sh/docs/topics/charts/#the-chartyaml-file)*

```yaml title="./deploy/xxxxx/xxxx/Chart.yaml"
apiVersion: v2
name: xxxx
description: A Helm chart for Kubernetes

# A chart can be either an 'application' or a 'library' chart.
#
# Application charts are a collection of templates that can be packaged into versioned archives
# to be deployed.
#
# Library charts provide useful utilities or functions for the chart developer. They're included as
# a dependency of application charts to inject those utilities and functions into the rendering
# pipeline. Library charts do not define any templates and therefore cannot be deployed.
type: application

# This is the chart version. This version number should be incremented each time you make changes
# to the chart and its templates, including the app version.
# Versions are expected to follow Semantic Versioning (https://semver.org/)
version: 0.1.0

# This is the version number of the application being deployed. This version number should be
# incremented each time you make changes to the application. Versions are not expected to
# follow Semantic Versioning. They should reflect the version the application is using.
appVersion: 1.16.0

dependencies:
- name: common
  version: 0.1.0
  repository: "file://../../library/common/"
```

If you can see with `Chart.yaml`, you could understand what meaning of Chart with important value like

- `apiVersion` : The chart API version **(required)** - *Default to use `helm3` with api `v2` to applied deployment*
- `name`: The name of the chart **(required)** - *This one will be effect to your kubernetes service, if name `xxxx`, for example your deployment name will be `xxxx`*
- `dependencies`: A list of the chart requirements **(optional)** - *Take the `common` with `pre-define` for mapping change `deployments and YAML manifests` for application.*

*NOTICE: Application will use file with relative path, make sure `../../library/common/` exist*

On CI progress, pipeline will trigger the action

```bash
helm dependency update ../xxxxx/charts/xxxx/
```

>[!done]
>With this action, your new chart application will create and publish artifact with
>- New value file if you change environment value
>- New variable of `secret` `configmap`, if you update them
>- Add new configuration like applying `HPA` to your application, for example

## CD (Continuous Delivery)

<h2>helm update </h2>

*Documentation: [doc](https://helm.sh/docs/helm/helm_upgrade/)*

**Purpose:  upgrade a release**

>[!info]
>This command upgrades a release to a new version of a chart.
>
>The upgrade arguments must be a release and chart. The chart argument can be either: a chart reference('example/mariadb'), a path to a chart directory, a packaged chart, or a fully qualified URL. For chart references, the latest version will be specified unless the `--version` flag is set.

Apply `--install` for purpose running both update and install if not exist `Chart` on Cluster

>[!question]
>You need to understand concept of `value-file` work with `helm update` to make sure not mistake occur when defining. 
>***NOTICE: The value will be set base on priority. Read [[Note about Helm#Priority of Value in Helm|Priority of Value in Helm]] to explain more***

1. All **environment with sensitive** need to be **masking** like *connection strings, password database, secret key, ...* by force to using `--set` flag to set this part on the pipeline.
2. Base on environment, you need to upgrading the specify value for each them like `production-values.yaml uat-values.yaml systemtest-values.yaml`
3. If you do not set any value, default `values.yaml` is specify for general value

On CD progress, pipeline will trigger the action

```bash
helm upgrade --install xxxx -f ../xxxxx/environments/systemtest-values.yaml  --set mongodb.password=$(MONGODB_PWD) ../xxxxx/charts/xxxx 
```

>[!done]
>With this action, If not wrong anything, your new application will release to K8s Cluster

## Another command

<h2>helm install</h2>

To install helm chart from self-define or community from [ArtifactHub](https://artifacthub.io/)

```
helm install <name-release> <chart-url>
```

<h2>helm list</h2>
To list releases on namespace

```bash
helm list -n <namespace>
```

<h2>helm repo</h2>
To add, list, remove, update, and index chart repositories

```bash
# Add a chart repository
helm repo add [NAME] [URL] [flags]

# Update information of available charts locally from chart repositories
helm repo update [REPO1 [REPO2 ...]] [flags]
```

<h2>helm uninstall</h2>

To uninstall a release

```bash
helm uninstall RELEASE_NAME [...] [flags]
```

# Debug Template

*Documentation: [Doc](https://helm.sh/docs/chart_template_guide/debugging/)*

>[!info]
>Debugging templates can be tricky because the rendered templates are sent to the Kubernetes API server, which may reject the YAML files for reasons other than formatting.
>
>There are a few commands that can help you debug.
>
>- `helm lint` is your go-to tool for verifying that your chart follows best practices
>- `helm template --debug` will test rendering chart templates locally.
>- `helm install --dry-run --debug` will also render your chart locally without installing it, but will also check if conflicting resources are already running on the cluster. Setting --dry-run=server will additionally execute any lookup in your chart towards the server.
>- `helm get manifest`: This is a good way to see what templates are installed on the server.

# Conclusion

>[!done]
>On this topic, you will learn and understand about helm for
>- Install `Helm` for your environment, for **debug** or **release**
>- Understand priority value of helm chart
>- How to use command with helm ?
>- Tools for helping you debug the template before apply for real environment
