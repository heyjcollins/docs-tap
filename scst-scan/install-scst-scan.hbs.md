# Install Supply Chain Security Tools - Scan

This topic describes how to install Supply Chain Security Tools - Scan
from the Tanzu Application Platform package repository.

> **Note** Follow the steps in this topic if you do not want to use a profile to install PACKAGE-NAME. For more information about profiles, see [About Tanzu Application Platform components and profiles](../about-package-profiles.hbs.md).

## <a id='scst-scan-prereqs'></a> Prerequisites

Before installing SCST - Scan:

- Complete all prerequisites to install Tanzu Application Platform. For more
  information, see [Prerequisites](../prerequisites.md).
- Install [Supply Chain Security Tools -
  Store](../scst-store/install-scst-store.md) for scan results to persist. The
  integration with SCST - Store are handled in:
  - **Single Cluster:** The SCST - Store is present in
    the same cluster where SCST - Scan and the
    `ScanTemplates` are present.
  - **Multi-Cluster:** The SCST - Store is present in a
    different cluster (e.g.: view cluster) where the SCST - Scan and `ScanTemplates` are present.
  - **Integration Deactivated:** The SCST - Scan
    deployment is not required to communicate with SCST -
    Store.

  For information about SCST - Store, see [Using the Supply Chain Security Tools - Store](../scst-store/overview.md).

## <a id='configure-scst-scan'></a> Configure properties

When you install the SCST - Scan (Scan controller), you can configure the following optional properties:

| Key | Default | Type | Description | ScanTemplate Version |
| --- | --- | --- | --- | --- |
| resources.limits.cpu | 250m | integer/string | Limits describes the maximum amount of CPU resources allowed. | _n/a_ |
| resources.limits.memory | 256Mi | integer/string | Limits describes the maximum amount of memory resources allowed. | _n/a_ |
| resources.requests.cpu | 100m | integer/string | Requests describes the minimum amount of CPU resources required. | _n/a_ |
| resources.requests.memory | 128Mi | integer/string | Requests describes the minimum amount of memory resources required. | _n/a_ |
| namespace | scan-link-system | string | Deployment namespace for the Scan Controller | _n/a_ |
| metadataStore.caSecret.importFromNamespace | metadata-store | string | Namespace from which you import the Insight Metadata Store CA Cert | earlier than v1.2.0 |
| metadataStore.caSecret.name | app-tls-cert | string | Name of deployed secret with key ca.crt holding the CA Cert of the Insight Metadata Store | earlier than v1.2.0 |
| metadataStore.clusterRole | metadata-store-read-write | string | Name of the deployed ClusterRole for read/write access to the Insight Metadata Store deployed in the same cluster | earlier than v1.2.0 |
| metadataStore.url | https://metadata-store-app.metadata-store.svc.cluster.local:8443 | string | URL of the Insight Metadata Store | earlier than v1.2.0 |
| metadataStore.authSecret.importFromNamespace | _n/a_ | string | Namespace from which to import the Insight Metadata Store auth_token | earlier than v1.2.0 |
| metadataStore.authSecret.name | _n/a_ | string | Name of deployed secret with key auth_token | earlier than v1.2.0 |
| retryScanJobsSecondsAfterError | 60 | integer | Seconds to wait before retrying errored scans | v1.3.1 and later |
| caCertData | "" | string | The custom certificates trusted by the scans' connections. | v1.4.0 and later |

When you install the SCST - Scan (Grype scanner), you can configure the following optional properties:

| Key | Default | Type | Description | ScanTemplate Version |
| --- | --- | --- | --- | --- |
| resources.requests.cpu | 250m | integer/string | Requests describes the minimum amount of CPU resources required. |
| resources.requests.memory | 128Mi | integer/string | Requests describes the minimum amount of memory resources required. | | 1000m | integer/string | Limits describes the maximum amount of CPU resources allowed. |
| scanner.serviceAccount | grype-scanner | string | Name of scan pod's service ServiceAccount |
| scanner.serviceAccountAnnotations | nil | object | Annotations added to ServiceAccount |
| targetImagePullSecret | _n/a_ | string | Reference to the secret used for pulling images from private registry |
| targetSourceSshSecret | _n/a_ | string | Reference to the secret containing SSH credentials for cloning private repositories |
| namespace | default | string | Deployment namespace for the Scan Templates | _n/a_ |
| metadataStore.url | https://metadata-store-app.metadata-store.svc.cluster.local:8443 | string | URL of the Insight Metadata Store | v1.2.0 and earlier |
| metadataStore.authSecret.name | _n/a_ | string | Name of deployed secret with key auth_token | v1.2.0 and earlier |
| metadataStore.authSecret.importFromNamespace | _n/a_ | string | Namespace from which to import the Insight Metadata Store auth_token | v1.2.0 and earlier |
| metadataStore.caSecret.importFromNamespace | metadata-store | string | Namespace from which to import the Insight Metadata Store CA Cert | v1.2.0 and earlier |
| metadataStore.caSecret.name | app-tls-cert | string | Name of deployed secret with key ca.crt holding the CA Cert of the Insight Metadata Store | v1.2.0 and earlier |
| metadataStore.clusterRole | metadata-store-read-write | string | Name of the deployed ClusterRole for read/write access to the Insight Metadata Store deployed in the same cluster | v1.2.0 |

## <a id='install-scst-scan'></a> Install
There are two options for installing Supply Chain Security Tools – Scan

### <a id='install-scst-scan-namespace-provisioner'></a> Option 1: Install to multiple namespaces with the Namespace Provisioner

The Namespace Provisioner enables operators to securely automate the provisioning of multiple developer namespaces in a shared cluster. To install Supply Chain Security Tools – Scan by using the Namespace Provisioner, see [Tutorial: Provisioning new developer namespaces](../namespace-provisioner/tutorials.hbs.md).

The Namespace Provisioner can also create scan policies across multiple developer namespaces. See [Add the resources required by the Out of the Box Testing and Scanning Supply Chain](../namespace-provisioner/how-tos.hbs.md#add-the-resources-required-by-the-out-of-the-box-testing-and-scanning-supply-chain) for configuration steps.

### <a id='install-scst-scan-manually'></a> Option 2: Install manually to each individual namespace
The installation for Supply Chain Security Tools – Scan involves installing two packages:

- Scan controller
- Grype scanner

The Scan controller enables you to use a scanner, in this case, the Grype
scanner. Ensure that both the Grype scanner and the Scan controller are installed.

To install SCST - Scan (Scan controller):

1. List version information for the package by running:

    ```console
    tanzu package available list scanning.apps.tanzu.vmware.com --namespace tap-install
    ```

     For example:

    ```console
    $ tanzu package available list scanning.apps.tanzu.vmware.com --namespace tap-install
    / Retrieving package versions for scanning.apps.tanzu.vmware.com...
      NAME                             VERSION       RELEASED-AT
      scanning.apps.tanzu.vmware.com   1.1.0
    ```

2. (Optional) Make changes to the default installation settings:

    If you're using the Grype Scanner `v1.2.0 and earlier`, or the Snyk Scanner, the
    following scanning configuration might deactivate the embedded SCST - Store integration with a `scan-values.yaml` file.

    ```yaml
    ---
    metadataStore:
      url: ""
    ```

    If you're using the Grype Scanner `earlier than 1.2.0`, the scanning configuration must
    configure the store parameters. See the [v1.1
    docs](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.1/tap/GUID-scst-scan-install-scst-scan.html)
    for reference.

    You can retrieve any other configurable setting using the following command,
    and appending the key-value pair to the previous `scan-values.yaml` file:

    ```console
    tanzu package available get scanning.apps.tanzu.vmware.com/VERSION --values-schema -n tap-install
    ```

    Where `VERSION` is your package version number. For example, `1.1.0`.

3. Install the package by running:

    ```console
    tanzu package install scan-controller \
      --package-name scanning.apps.tanzu.vmware.com \
      --version VERSION \
      --namespace tap-install \
      --values-file scan-values.yaml
    ```

    Where `VERSION` is your package version number. For example, `1.1.0`.

<a id="install-grype"></a> To install SCST - Scan (Grype scanner):

1. List version information for the package by running:

    ```console
    tanzu package available list grype.scanning.apps.tanzu.vmware.com --namespace tap-install
    ```

    For example:

    ```console
    $ tanzu package available list grype.scanning.apps.tanzu.vmware.com --namespace tap-install
    / Retrieving package versions for grype.scanning.apps.tanzu.vmware.com...
      NAME                                  VERSION       RELEASED-AT
      grype.scanning.apps.tanzu.vmware.com  1.1.0
    ```

2. (Optional) Make changes to the default installation settings:

    To define the configuration for the SCST - Store
    integration in the `grype-values.yaml` file for the Grype Scanner:

    ```yaml
    ---
    namespace: "DEV-NAMESPACE" # The developer namespace where the ScanTemplates are gonna be deployed
    metadataStore:
      url: "METADATA-STORE-URL" # The base URL where the Store deployment can be reached
      caSecret:
        name: "CA-SECRET-NAME" # The name of the secret containing the ca.crt
        importFromNamespace: "SECRET-NAMESPACE" # The namespace where Store is deployed (if single cluster) or where the connection secrets were created (if multi-cluster)
      authSecret:
        name: "TOKEN-SECRET-NAME" # The name of the secret containing the auth token to connect to Store
        importFromNamespace: "SECRET-NAMESPACE" # The namespace where the connection secrets were created (if multi-cluster)
    ```
    >**Note** You must either define both the `METADATA-STORE-URL` and `CA-SECRET-NAME`,
    >or not define them as they depend on each other.

    Where:

    - `DEV-NAMESPACE` is the namespace where you want to deploy the
    `ScanTemplates`. This is the namespace where the scanning feature runs.
    - `METADATA-STORE-URL` is the base URL where the Supply Chain Security Tools
      (SCST) - Store deployment is reached, for example,
      `https://metadata-store-app.metadata-store.svc.cluster.local:8443`.
    - `CA-SECRET-NAME` is the name of the secret containing the ca.crt to
      connect to the SCST - Store deployment.
    - `SECRET-NAMESPACE` is the namespace where SCST - Store is deployed, if you
      are using a single cluster. If you are using multicluster, it is where the
      connection secrets were created.
    - `TOKEN-SECRET-NAME` is the name of the secret containing the
    authentication token to connect to the SCST - Store deployment when
    installed in a different cluster, if you are using multicluster. If built
    images are pushed to the same registry as the Tanzu Application Platform
    images, this can reuse the `tap-registry` secret created in [Add the Tanzu
    Application Platform package
    repository](../install.html#add-tap-package-repo) as described earlier.

    You can retrieve any other configurable setting using the following command,
    and appending the key-value pair to the previous `grype-values.yaml` file:

    ```console
    tanzu package available get grype.scanning.apps.tanzu.vmware.com/VERSION --values-schema -n tap-install
    ```

    Where `VERSION` is your package version number. For example, `1.1.0`.

    For example:

    ```console
    $ tanzu package available get grype.scanning.apps.tanzu.vmware.com/1.1.0 --values-schema -n tap-install
    | Retrieving package details for grype.scanning.apps.tanzu.vmware.com/1.1.0...
      KEY                        DEFAULT  TYPE    DESCRIPTION
      namespace                  default  string  Deployment namespace for the Scan Templates
      resources.limits.cpu       1000m    <nil>   Limits describes the maximum amount of cpu resources allowed.
      resources.requests.cpu     250m     <nil>   Requests describes the minimum amount of cpu resources required.
      resources.requests.memory  128Mi    <nil>   Requests describes the minimum amount of memory resources required.
      targetImagePullSecret      <EMPTY>  string  Reference to the secret used for pulling images from private registry.
      targetSourceSshSecret      <EMPTY>  string  Reference to the secret containing SSH credentials for cloning private repositories.
    ```

    >**Note** If `targetSourceSshSecret` is not set, the private source scan template is not installed.

3. Install the package by running:

    ```console
    tanzu package install grype-scanner \
      --package-name grype.scanning.apps.tanzu.vmware.com \
      --version VERSION \
      --namespace tap-install \
      --values-file grype-values.yaml
    ```

    Where `VERSION` is your package version number. For example, `1.1.0`.

    For example:

    ```console
    $ tanzu package install grype-scanner \
      --package-name grype.scanning.apps.tanzu.vmware.com \
      --version 1.1.0 \
      --namespace tap-install \
      --values-file grype-values.yaml
    / Installing package 'grype.scanning.apps.tanzu.vmware.com'
    | Getting namespace 'tap-install'
    | Getting package metadata for 'grype.scanning.apps.tanzu.vmware.com'
    | Creating service account 'grype-scanner-tap-install-sa'
    | Creating cluster admin role 'grype-scanner-tap-install-cluster-role'
    | Creating cluster role binding 'grype-scanner-tap-install-cluster-rolebinding'
    / Creating package resource
    - Package install status: Reconciling

     Added installed package 'grype-scanner' in namespace 'tap-install'
    ```
