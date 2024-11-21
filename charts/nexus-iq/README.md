# Sonatype IQ server

[Sonatype Nexus IQ Server](https://www.sonatype.com/nexus-iq-server)  is everything you need to know to trust your software supply chain. It powers Nexus Firewall, Nexus Lifecycle, and Nexus Auditor.

### Prerequisites

- Kubernetes 1.19+
- PV provisioner support in the underlying infrastructure
- Helm 3

These charts are designed to work out of the box with minikube using both ingess and ingress-dns addons.

The current releases have been tested on minikube v1.25.1 running Kubernetes v1.23.1.

## Adding the Sonatype Repository to your Helm

To Add as a Helm Repo
```helm repo add sonatype https://sonatype.github.io/helm3-charts/```

## Testing the Chart

To test the chart:
```bash
$ helm install --dry-run --debug --generate-name ./
```
To test the chart with your own values:
```bash
$ helm install --dry-run --debug --generate-name -f myvalues.yaml ./ 
```

## Installing the Chart

To install the chart:

```bash
$ helm install nexus-iq sonatype/nexus-iq-server [ --version v90.0.0 ]
```

The above command deploys IQ on the Kubernetes cluster in the default configuration. Note the optional version flag.

You can pass custom configuration values as:

```bash
$ helm install -f myvalues.yaml ./ --name nexus-iq
```

The default login for the IQ Server is admin/admin123

## Upgrading the Chart

```bash
$ helm upgrade nexus-iq sonatype/nexus-iq-server [--version v91.0.0]
```


Note: 
 - optional version flag shown.
 - When upgrading from IQ versions 1.117 or 1.100, set fixOwner.enabled to true for changing the file ownership. 

## Uninstalling the Chart

To uninstall/delete the deployment:

```bash
$ helm list
NAME                    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                   APP VERSION
plinking-gopher	        default         1               2021-03-10 15:13:04.614125 -0800 PST    deployed        nexus-iq-server-106.0.0 1.106.0    
$ helm delete plinking-gopher
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Chart Configuration Options

| Parameter                                      | Description                                                                                                                                                                             | Default                                                     |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------|
| `image.name`                                   | The image name to use for the IQ Container                                                                                                                                              | `sonatype/nexus-iq-server`                                  |
| `image.tag`                                    | The version/tag to use for the IQ Container                                                                                                                                             | See `values.yaml`                                           |
| `imagePullSecrets`                             | The names of the kubernetes secrets with credentials to login to a registry                                                                                                             | `[]`                                                        |
| `iq.applicationPort`                           | Port of the application connector. Must match the value in the `configYaml` property                                                                                                    | `8070`                                                      |
| `iq.adminPort`                                 | Port of the application connector. Must match the value in the `configYaml` property                                                                                                    | `8071`                                                      |
| `iq.licenseSecret`                             | The base-64 encoded license file to be installed at startup                                                                                                                             | `""`                                                        |
| `iq.env`                                       | IQ server environment variables, including JAVA_OPTS                                                                                                                                    | See `values.yaml`                                           |
| `iq.secretName`                                | The name of a secret to mount inside the container                                                                                                                                      | See `values.yaml`                                           |
| `iq.secretMountName`                           | Where in the container to mount the data from `secretName`                                                                                                                              | See `values.yaml`                                           |
| `iq.livenessProbe.initialDelaySeconds`         | LivenessProbe initial delay                                                                                                                                                             | 10                                                          |
| `iq.livenessProbe.periodSeconds`               | Seconds between polls                                                                                                                                                                   | 10                                                          |
| `iq.livenessProbe.failureThreshold`            | Number of attempts before failure                                                                                                                                                       | 3                                                           |
| `iq.livenessProbe.timeoutSeconds`              | Time in seconds after liveness probe times out                                                                                                                                          | 2                                                           |
| `iq.livenessProbe.successThreshold`            | Number of attempts for the probe to be considered successful                                                                                                                            | 1                                                           |
| `iq.readinessProbe.initialDelaySeconds`        | ReadinessProbe initial delay                                                                                                                                                            | 10                                                          |
| `iq.readinessProbe.periodSeconds`              | Seconds between polls                                                                                                                                                                   | 10                                                          |
| `iq.readinessProbe.failureThreshold`           | Number of attempts before failure                                                                                                                                                       | 3                                                           |
| `iq.readinessProbe.timeoutSeconds`             | Time in seconds after readiness probe times out                                                                                                                                         | 2                                                           |
| `iq.readinessProbe.successThreshold`           | Number of attempts for the probe to be considered successful                                                                                                                            | 1                                                           |
| `fixOwner.enabled`                       | Change file ownership during IQ upgrade from 1.117 or 1.100 versions                                                                                                                    | false                                                       |
| `configYaml`                                   | A YAML block which will be used as a configuration block for IQ Server.                                                                                                                 | See `values.yaml`                                           |
| `ingress.enabled`                              | Create an ingress for Nexus                                                                                                                                                             | `false`                                                     |
| `ingress.annotations`                          | Annotations to enhance ingress configuration                                                                                                                                            | `{}`                                                        |
| `ingress.tls.enabled`                          | Enable TLS                                                                                                                                                                              | `true`                                                      |
| `ingress.tls.secretName`                       | Name of the secret storing TLS cert, `false` to use the Ingress' default certificate                                                                                                    | `nexus-tls`                                                 |
| `ingress.path`                                 | Path for ingress rules. GCP users should set to `/*`                                                                                                                                    | `/`                                                         |
| `deployment.preStart.command`                  | Command to run before starting the IQ Server container                                                                                                                                  | `nil`                                                       |
| `deployment.postStart.command`                 | Command to run after starting the IQ Server container                                                                                                                                   | `nil`                                                       |
| `deployment.terminationGracePeriodSeconds`     | Update termination grace period (in seconds)                                                                                                                                            | 120s                                                        |
| `persistence.storageClass`                     | The provisioner class                                                                                                                                                                   | `-` (disables dynamic provisioning)                         |
| `persistence.dataStorageSize`                  | The amount of drive space to allocate for data                                                                                                                                          | `1Gi`                                                       |
| `persistence.logStorageSize`                   | The amount of drive space to allocate for logs                                                                                                                                          | `1Gi`                                                       |
| `persistence.accessMode`                       | Default access mode                                                                                                                                                                     | `ReadWriteOnce`                                             |
| `persistence.existingClaim`                    | Pre-created PVC name for Data Volume                                                                                                                                                    | `nil`                                                       |
| `persistence.existingLogClaim`                 | Pre-created PVC name for Log Volume                                                                                                                                                     | `nil`                                                       |
| `persistence.pdName` **DEPRECATED**            | Moved to  `persistence.gcePersistentDisk.pdName`                                                                                                                                        | NA                                                          |
| `persistence.fsType` **DEPRECATED**            | Moved to  `persistence.gcePersistentDisk.fsType`                                                                                                                                        | NA                                                          |
| `persistence.pvName`                           | The name for the persistentVolume being created to hold IQ Data                                                                                                                         | `nil`                                                       |
| `persistence.logPVName`                        | The name for the persistentVolume being created to hold IQ Logs                                                                                                                         | `nil`                                                       |
| `persistence.gcePersistentDisk`                | A block for using existing gcePersistentDisks                                                                                                                                           | `nil`                                                       |
| `persistence.gcePersistentDisk.pdName`         | GCE PersistentDisk to use for IQ Data                                                                                                                                                   | `nil`                                                       |
| `persistence.gcePersistentDisk.fsType`         | File system type for the IQ Data disk                                                                                                                                                   | `nil`                                                       |
| `persistence.gcePersistentDisk.logPDName`      | GCE PersistentDisk to use for IQ Logs                                                                                                                                                   | `nil`                                                       |
| `persistence.gcePersistentDisk.logFSType`      | File system type for the IQ Logs disk                                                                                                                                                   | `nil`                                                       |
| `persistence.awsElasticBlockStore`             | A block for using existing AWS EBS Volumes                                                                                                                                              | `nil`                                                       |
| `persistence.awsElasticBlockStore.volumeID`    | AWS EBS Volume to use for IQ Data                                                                                                                                                       | `nil`                                                       |
| `persistence.awsElasticBlockStore.fsType`      | File system type for the IQ Data disk                                                                                                                                                   | `nil`                                                       |
| `persistence.awsElasticBlockStore.logVolumeID` | AWS EBS Volume to use for IQ Logs                                                                                                                                                       | `nil`                                                       |
| `persistence.awsElasticBlockStore.logFSType`   | File system type for the IQ Logs disk                                                                                                                                                   | `nil`                                                       |
| `persistence.csi`                              | A YAML block for defining CSI Storage Driver configuration for the Data PV. The entire block is taken as you write it. Should support _any_ csi driver that your cluster has installed. | `nil`                                                       |
| `persistence.logCSI`                           | A YAML block for defining CSI Storage Driver configuration for the Log PV. The entire block is taken as you write it. Should support _any_ csi driver that your cluster has installed.  | `nil`                                                       |
| `persistence.affinity.nodeSelectorTerms`       | A YAML block for defining the affinity node selection. This block is taken as you write it.                                                                                             | `nil`                                                       |
| `resources`                                    | Resource requests and limits for the IQ pod in the cluster.                                                                                                                             | See `values.yaml` for suggested minimum recommended values. |
| `podAnnotations`                               | Annotations to add to iq server pod.                                                                                                                                                    | `nil`                                                       |
| `podSecurityContext`                           | `securityContext` for the whole pod.                                                                                                                                                    | Default matches the stock container.                        |

## Configuring IQ Server

You can define the `config.yml` for IQ Server in your `myvalues.yml` file on startup. 
It is the `configYaml` property. For more details, see the [Configuring IQ Server](https://help.sonatype.com/iqserver/configuring) help page.
Additionally the server can be started with JAVA_OPTS exported to the environment. This will be added to the server 
process invocation and can be used for purposes such as changing the server memory settings. See the defaults set in
[the values.yaml file](values.yaml).

## Installing the License

The license file can be installed via the UI when IQ server is running, or it can be done as a part of the deploy. 
If you leave the `licenseFile` field empty/commented, IQ Server will start and prompt you to manually install the license 
when you first enter the GUI.

## Specifying custom Java keystore/truststore

There is an example of how to implement this in [the values.yaml file](values.yaml) using secrets to store both the
Java keystores and their associated passwords. In order to utilize the provided example directly secrets can be created 
from a directory containing the keystore and truststore files like so:
```shell
kubectl create secret generic secret-jks 
	--from-file=truststore.jks=./truststore.jks 
	--from-file=keystore.jks=./keystore.jks 
	--from-literal='keystorePassword=password' 
	--from-literal='truststorePassword=password'
```

## Using the Image from the Red Hat Registry

To use the [IQ image available from Red Hat's registry](https://catalog.redhat.com/software/containers/sonatype/nexus-iq-server/5e5d8063ac3db90370816c66),
you'll need to:
* Load the credentials for the registry as a secret in your cluster
  ```shell
  kubectl create secret docker-registry redhat-pull-secret \
    --docker-server=registry.connect.redhat.com \
    --docker-username=<user_name> \
    --docker-password=<password> \
    --docker-email=<email>
  ```
  See Red Hat's [Registry Authentication documentation](https://access.redhat.com/RegistryAuthentication)
  for further details.
* Provide the name of the secret in `imagePullSecrets` in this chart's `values.yaml`
  ```yaml
  imagePullSecrets:
    - name: redhat-pull-secret
  ```
* Set `image.name` and `image.tag` in `values.yaml`
  ```yaml
  image:
    repository: registry.connect.redhat.com/sonatype/nexus-iq-server
    tag: 1.132.0-ubi-1
  ```
