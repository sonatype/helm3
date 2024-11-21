## Helm3 Charts for Sonatype Products
### AWS Single-Instance Resilient Nexus Repository Manager Chart
These charts will deploy a Nexus Repository Manager instance to an AWS EKS cluster.

The current release has been tested on AWS EKS running Kubernetes version 1.21

### Single-Instance Nexus Repository Manager OSS/Pro Kubernetes Chart
These charts are designed to work out of the box with minikube using both Ingress
and Ingress DNS addons.

The current releases have been tested on minikube v1.25.1 running Kubernetes v1.23.1.

### Add the Sonatype Repo to Your Helm

`helm repo add sonatype https://sonatype.github.io/helm3-charts/`

### Install a Server

- Single-Instance Nexus Repository Manager OSS/Pro: `helm install nexus-repo sonatype/nexus-repository-manager`
- AWS Single-Instance Resilient Nexus Repository Manager: `helm install nxrm sonatype/nxrm-aws-resiliency --values values.yaml`
- Nexus IQ: `helm install nexus-iq sonatype/nexus-iq-server`

### Get the Values for Configuring a Chart

- Single-Instance Nexus Repository Manager OSS/Pro: `helm show values sonatype/nexus-repository-manager`
- AWS Single-Instance Resilient Nexus Repository Manager: `helm show values sonatype/nxrm-aws-resiliency`
- Nexus IQ: `helm show values sonatype/nexus-iq-server`

Capture that output as your own `values.yaml` file, and provide it to the `helm install` 
command with the `-f` option.

### Source

See the [`sonatype/helm3-charts`](https://github.com/sonatype/helm3-charts) repo.
