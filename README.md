[![Build Status](https://travis-ci.org/containerum/containerum.svg?branch=master)](https://travis-ci.org/containerum/containerum) [![GitHub release](https://img.shields.io/github/release/containerum/containerum.svg)](https://github.com/containerum/containerum/releases) [![Twitter Follow](https://img.shields.io/twitter/follow/containerumcom.svg?style=social&label=Follow)](https://twitter.com/Containerumcom)


![Containerum logo](logo.svg)

## Overview

Containerum is a cloud management sistem. Containerum includes tools for simple CI/CD pipelines setup and team management. Release notes are available [here](https://docs.containerum.com/release-notes/platform/).

- Runs on top of any Kubernetes cluster
- Imports the resources already existing in Kubernetes clusters

![web-ui](/web-ui.png)

Containerum UI consists of several components:

* [**api-gateway**](https://github.com/containerum/gateway) provides routing for Containerum components
* [**user-manager**](https://github.com/containerum/user-manager) is a service for managing users, groups, credentials for Containerum
* [**resource**](https://github.com/containerum/resource) manages Kubernetes namespace objects: deployments, ingresses, etc.
* [**permissions**](https://github.com/containerum/permissions) manage user access to enable teamwork
* [**kube-api**](https://github.com/containerum/kube-api) is a set of API for communication between Containerum and K8s
* [**auth**](https://github.com/containerum/auth) handles user authorization and token management
* [**mail**](https://github.com/containerum/mail) is a mail server and newsletter template manager
* [**ui**](https://github.com/containerum/ui) is the GUI for Containerum
* [**solutions**](https://github.com/containerum/solutions) is a service for launching pre-built application configurations
* [**volume-manager**](https://github.com/containerum/volume-manager) is a storage class manager
* [**nodeMetrics**](https://github.com/containerum/nodeMetrics) is a Prometheus-based service for monitoring the node resource utilization
* [**events-api**](https://github.com/containerum/events-api) is an API that formats events data from the DB and passes it to Containerum
* [**kube-events**](https://github.com/containerum/kube-events) sources events from the Kubernetes cluster and writes them in the database
* [**kube-importer**](https://github.com/containerum/kube-importer) is a service that imports pre-existing Kubernetes objects to Containerum
* [**chkit**](https://github.com/containerum/chkit) is a CLI for Containerum

and 2 databases:
* MongoDB
* PostgreSQL

Basically, Containerum architecture looks like this:

![Containerum_components](components.svg)


## Prerequisites
Before installing Containerum make sure you have the following components:

* Docker
* Kubernetes *1.9 or higher*
* Helm
* Installed [Kubernetes Ingress Controller](ingress.md)

## How to install
To launch Containerum on your Kubernetes Cluster without metrics collection run:

```
helm repo add containerum https://charts.containerum.io
helm repo update
helm install containerum/containerum
```

To enable collecting resource utilization metrics, install Containerum with Prometheus:

```
helm repo add containerum https://charts.containerum.io
helm repo update
helm install containerum/containerum —-set tags.monitoring=true
```

If you already have Prometheus in your cluster and want to use it to display node utilization in Containerum Web UI, install Containerum with the parameters below. Containerum is compatible with Prometheus `6.7.4` from the official Helm repository.

```
helm repo add containerum https://charts.containerum.io
helm repo update
helm install containerum/containerum —-set nodemetrics.env.local.PROMETHEUS_ADDR=http://{PROMETHEUS_SVC_NAME}:{PROMETHEUS_SVC_PORT}
```

 This will install Containerum and create two Ingresses to expose it. You can view the Ingresses with `kubectl get ingress`.

> Note 1: Containerum allows importing existing resources (deployments, services, etc.). To specify, which namespaces SHOULD NOT be imported, set the following tag during installation and list the namespaces after `CH_KUBE_IMPORTER_EXCLUDED_NS=`.
 ```
--set kube-importer.env.global.CH_KUBE_IMPORTER_EXCLUDED_NS="default,kube-system"
```


> Note 2: To create deployments in Containerum you need to have an application node. In case you use only one node, make sure it is labeled as `slave`.  To add the label, run:  

```
kubectl label node ubuntu-01 role=slave
```
where `ubuntu-01` is the name of your node.  

To be able to reach Containerum Web UI and the API, add the machine IP address to /etc/hosts, e.g.:

```
127.0.0.1 local.containerum.io api.local.containerum.io
```

where ```127.0.0.1``` is the address of your machine with Containerum.

Now you can access Containerum Web UI at ```local.containerum.io```.

Default username: admin@local.containerum.io

Password: verystrongpassword

## Getting started
Containerum has an intuitive interface. Yet we recommend starting with learning the [object types](https://docs.containerum.com/objects/object-types/) in Containerum as well as the instructions on configuring [Containerum](https://docs.containerum.com/configuration/) in our Docs.

Don't forget to install [chkit CLI](https://github.com/containerum/chkit) (from source code or binaries - whatever you prefer!).

## Roadmap
To keep track of roadmap implementation, please refer to [Containerum Projects page](https://github.com/containerum/containerum/projects).


- Add events collection and notifications

- Add custom Dashboards

- Add integration with Jira

- Add integration with GitLab

- Add reporting tools

- Add project visualization

- Create scripts to setup CI/CD Pipelines for
	- Jenkins
	- TeamCity
	- Bamboo
	- GitLab
	- Travis
	- Codeship
	- Add pipeline management to chkit
 	- Add pipeline visualization to UI

- ~~Add node utilisation charts to the Dashboard~~


- Add Solutions and Marketplace
	- ~~Add Solutions~~
	- Add Marketplace

- ~~Add integration with Helm~~

- Implement ‘Project’ entity
	- ~~Add Projects~~
	- Add user groups to Projects
	- ~~Add users to Projects~~
	- Add role management to Projects
	- Add role management to stage environments
	- Add Projects to UI (CRUD)
	- Add recurring checking of completed fields
	- Create Events Server to track events in Projects

- Create Stage environments

- Implement revision control for stage environments
	- Add revision history within a stage
	- Add version change confirmation in production
	- Add revision control
	- Add automated rollbacks
	- Implement rollback mechanism
	- Implement update of changed files only

- Implement revision control for Projects

- ~~Add admin control features~~


## Contributing
We welcome any help from the open source community. To submit your contributions, fork the project you want to contribute to (e.g. *permissions, auth, etc.*), commit changes and create a pull request to the develop branch. We will review the changes and include them to the project. Read more about contributing [in this document.](https://github.com/containerum/containerum/blob/master/CONTRIBUTING.md)

## Issues and Support
Please submit any comments and report Containerum project bugs or issues in this repository.
In case you have questions about Containerum, there are several ways you can reach out to our team: by filling out the form on our [support page](https://containerum.com/support/) or by email at support@containerum.io.

## Docs
You can find the Documentation in the [Docs section](https://docs.containerum.com/) on our website or in the [Docs repository](https://github.com/containerum/containerum-docs) here on GutHub. Please, feel free to contribute to the Docs to help us make them more accurate and comprehensive.

## License
Copyright (c) 2015-2018 Exon LV.

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
