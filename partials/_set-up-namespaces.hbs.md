# Set up developer namespaces to use installed packages

You can choose either one of the following two approaches to create a `Workload`
for your application by using the registry credentials specified, 
add credentials and Role-Based Access Control (RBAC) rules to the namespace
that you plan to create the `Workload` in:

- [Enable single user access](#single-user-access).
- [Enable additional users access with Kubernetes RBAC](#additional-user-access).

If you plan to install Out of the Box Supply Chain with testing and scanning, follow additional steps to configure the developer namespace for that use case.

## <a id='single-user-access'></a>Enable single user access

Follow these steps to enable your current user to submit workloads to the Supply Chain:

As of TAP v1.4.0, `Namespace Provisioner` automatically handles everything for you.</br>
Add the label `namespaces ns1 apps.tanzu.vmware.com/tap-ns` to any namespace you'd like to set up
and `Namespace Provisioner` will take care of the rest.</br>

>**Note** `Namespace Provisioner` acts on any namespace with the label `namespaces ns1 apps.tanzu.vmware.com/tap-ns` applied regardless of the label selector's value (even an empty string). 

To create and set up a namespace called `ns1` execute the following:</br>
1. create and label the namespace:
    ```console
    kubectl create namespace ns1
    kubectl label namespaces ns1 apps.tanzu.vmware.com/tap-ns=""
    ```
2. (**Conditionally Optional**) - The `registry-credentials` secret, which is referenced by the Tanzu Build Service, is typically added during TAP installation at which time it gets exported to all namespaces. This secret is required for `Namespace Provisioner` to work properly.</br>
If this secret wasn't exported to all namespaces for any reason, you will have to manually create this secret in namespace `ns1`.
  * check to see if `registry-credentials` was created and exported to all namespaces:
    ```console
    tanzu secret registry list -n tap-install
    ```
  * Look for `registry-credentials` in the output from the command above.</br>
  If the `EXPORTED` value _**is not**_ `to all namespaces`, you must create the secret in namespace `ns1`:
    ```console
    tanzu secret registry add registry-credentials \
    --server REGISTRY-SERVER --username REGISTRY-USERNAME \
    --password REGISTRY-PASSWORD --namespace ns1 --yes
    ```

Unless you need to [enable additional users access with Kubernetes RBAC](#additional-user-access) after you've followed the instructions above, you're done!

Additonal instructions on how to customize and extend `namespace provisioner` will be published shortly.


---------------
### Legacy namespace set up instructions 

**These should be removed once TAP 1.4 docs are made public**
If you're running an older version of TAP which doesn't include `Namespace Provisioner` you can follow the directions below:

1. To add read/write registry credentials to the developer namespace, run:

   ```console
   tanzu secret registry add registry-credentials --server REGISTRY-SERVER --username REGISTRY-USERNAME 
   --password REGISTRY-PASSWORD --namespace YOUR-NAMESPACE
   ```

   Where:

    - `YOUR-NAMESPACE` is the name you give to the developer namespace.
    For example, use `default` for the default namespace.
    - `REGISTRY-SERVER` is the URL of the registry. For Docker Hub, this must be
    `https://index.docker.io/v1/`. Specifically, it must have the leading `https://`, the `v1` path,
    and the trailing `/`. For Google Container Registry (GCR), this is `gcr.io`.
    Based on the information used in [Installing the Tanzu Application Platform Package and Profiles](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/{{ vars.url_version }}/tap/GUID-install.html), you can use the
    same registry server as in `ootb_supply_chain_basic` - `registry` - `server`.
    - `REGISTRY-PASSWORD` is the password of the registry.
    For GCR or Google Artifact Registry, this must be the concatenated version of the JSON key. For example: `"$(cat ~/gcp-key.json)"`.

    If you observe the following issue with the above command:

    ```console
    panic: runtime error: invalid memory address or nil pointer dereference
    [signal SIGSEGV: segmentation violation code=0x1 addr=0x128 pc=0x2bcce00]
    ```

    Use `kubectl` to create the secret:

    ```console
    kubectl create secret docker-registry registry-credentials --docker-server=REGISTRY-SERVER --docker-username=REGISTRY-USERNAME --docker-password=REGISTRY-PASSWORD -n YOUR-NAMESPACE
    ```

    >**Note** This step is not required if you install Tanzu Application Platform on AWS with EKS and use [IAM Roles for Kubernetes Service Accounts](https://docs.aws.amazon.com/eks/latest/userguide/iam-roles-for-service-accounts.html) instead of secrets. You can specify the Role Amazon Resource Name (ARN) in the next step.

1. To add secrets, a service account to execute the supply chain, and RBAC rules to authorize the service account to the developer namespace, run:

    ```console
    cat <<EOF | kubectl -n YOUR-NAMESPACE apply -f -
    apiVersion: v1
    kind: Secret
    metadata:
      name: tap-registry
      annotations:
        secretgen.carvel.dev/image-pull-secret: ""
    type: kubernetes.io/dockerconfigjson
    data:
      .dockerconfigjson: e30K
    ---
    apiVersion: v1
    kind: ServiceAccount
    metadata:
      name: default
    secrets:
      - name: registry-credentials
    imagePullSecrets:
      - name: registry-credentials
      - name: tap-registry
    ---
    apiVersion: rbac.authorization.k8s.io/v1
    kind: RoleBinding
    metadata:
      name: default-permit-deliverable
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: deliverable
    subjects:
      - kind: ServiceAccount
        name: default
    ---
    apiVersion: rbac.authorization.k8s.io/v1
    kind: RoleBinding
    metadata:
      name: default-permit-workload
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: workload
    subjects:
      - kind: ServiceAccount
        name: default
    EOF
    ```

    >**Note** If you install Tanzu Application Platform on AWS with EKS and use [IAM Roles for Kubernetes Service Accounts](https://docs.aws.amazon.com/eks/latest/userguide/iam-roles-for-service-accounts.html), you must annotate the ARN of the IAM Role and remove the `registry-credentials` secret. Your service account entry then looks like the following:

    ```
    apiVersion: v1
    kind: ServiceAccount
    metadata:
      name: default
      annotations:
        eks.amazonaws.com/role-arn: <Role ARN>
    imagePullSecrets:
      - name: tap-registry
    ```

## <a id='additional-user-access'></a>Enable additional users access with Kubernetes RBAC

Follow these steps to enable additional users by using Kubernetes RBAC to submit jobs to the Supply Chain:

1. [Enable single user access](#single-user-access).

1. Choose either of the following options to give developers namespace-level access and view access to appropriate cluster-level resources:

    - **Option 1:** Use the [Tanzu Application Platform RBAC CLI plug-in (beta)](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/{{ vars.url_version }}/tap/GUID-authn-authz-binding.html#install-the-tanzu-application-platform-rbac-cli-plugin-1).

        To use the `tanzu rbac` plug-in to grant `app-viewer` and `app-editor` roles to an identity provider group, run:

        ```console
        tanzu rbac binding add -g GROUP-FOR-APP-VIEWER -n YOUR-NAMESPACE -r app-viewer
        tanzu rbac binding add -g GROUP-FOR-APP-EDITOR -n YOUR-NAMESPACE -r app-editor
        ```

        Where:

        - `YOUR-NAMESPACE` is the name you give to the developer namespace.
        - `GROUP-FOR-APP-VIEWER` is the user group from the upstream identity provider that requires access to `app-viewer` resources on the current namespace and cluster.
        - `GROUP-FOR-APP-EDITOR` is the user group from the upstream identity provider that requires access to `app-editor` resources on the current namespace and cluster.

        For more information about `tanzu rbac`, see
        [Bind a user or group to a default role](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/{{ vars.url_version }}/tap/GUID-authn-authz-binding.html).

        VMware recommends creating a user group in your identity provider's grouping system for each
        developer namespace and then adding the users accordingly.

        Depending on your identity provider, you might need to take further action to
        federate user groups appropriately with your cluster.
        For an example of how to set up Azure Active Directory (AD) with your cluster, see
        [Integrating Azure Active Directory](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/{{ vars.url_version }}/tap/GUID-authn-authz-azure-ad.html).

    - **Option 2:** Use the native Kubernetes YAML.

        To apply the RBAC policy, run:

        ```console
        cat <<EOF | kubectl -n YOUR-NAMESPACE apply -f -
        apiVersion: rbac.authorization.k8s.io/v1
        kind: RoleBinding
        metadata:
          name: dev-permit-app-viewer
        roleRef:
          apiGroup: rbac.authorization.k8s.io
          kind: ClusterRole
          name: app-viewer
        subjects:
          - kind: Group
            name: GROUP-FOR-APP-VIEWER
            apiGroup: rbac.authorization.k8s.io
        ---
        apiVersion: rbac.authorization.k8s.io/v1
        kind: ClusterRoleBinding
        metadata:
          name: YOUR-NAMESPACE-permit-app-viewer
        roleRef:
          apiGroup: rbac.authorization.k8s.io
          kind: ClusterRole
          name: app-viewer-cluster-access
        subjects:
          - kind: Group
            name: GROUP-FOR-APP-VIEWER
            apiGroup: rbac.authorization.k8s.io
        ---
        apiVersion: rbac.authorization.k8s.io/v1
        kind: RoleBinding
        metadata:
          name: dev-permit-app-editor
        roleRef:
          apiGroup: rbac.authorization.k8s.io
          kind: ClusterRole
          name: app-editor
        subjects:
          - kind: Group
            name: GROUP-FOR-APP-EDITOR
            apiGroup: rbac.authorization.k8s.io
        ---
        apiVersion: rbac.authorization.k8s.io/v1
        kind: ClusterRoleBinding
        metadata:
          name: YOUR-NAMESPACE-permit-app-editor
        roleRef:
          apiGroup: rbac.authorization.k8s.io
          kind: ClusterRole
          name: app-editor-cluster-access
        subjects:
          - kind: Group
            name: GROUP-FOR-APP-EDITOR
            apiGroup: rbac.authorization.k8s.io
        EOF
        ```

        Where:

        - `YOUR-NAMESPACE` is the name you give to the developer namespace.
        - `GROUP-FOR-APP-VIEWER` is the user group from the upstream identity provider that requires access to `app-viewer` resources on the current namespace and cluster.
        - `GROUP-FOR-APP-EDITOR` is the user group from the upstream identity provider that requires access to `app-editor` resources on the current namespace and cluster.

        VMware recommends creating a user group in your identity provider's grouping system for each
        developer namespace and then adding the users accordingly.

        Depending on your identity provider, you might need to take further action to
        federate user groups appropriately with your cluster.
        For an example of how to set up Azure Active Directory (AD) with your cluster, see
        [Integrating Azure Active Directory](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/{{ vars.url_version }}/tap/GUID-authn-authz-azure-ad.html).

        Rather than granting roles directly to individuals, VMware recommends using your identity provider's user groups system to grant access to a group of developers.
        For an example of how to set up Azure AD with your cluster, see
        [Integrating Azure Active Directory](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/{{ vars.url_version }}/tap/GUID-authn-authz-azure-ad.html).

1. (Optional) Log in as a non-admin user, such as a developer, to see the effects of RBAC after the bindings are applied.

## Additional configuration for testing and scanning

If you plan to install Out of the Box Supply Chains with Testing and Scanning, see the [Developer Namespace](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/{{ vars.url_version }}/tap/GUID-scc-ootb-supply-chain-testing.html#developer-namespace-1) section.
