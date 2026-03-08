# Local full-featured Kubernetes development clusters

You can use this repository to create preconfigured GitOps Kubernetes clusters. Connect your local git repository and create commits for updating and customizing the cluster.

## Components

- [Cilium](https://cilium.io/)
- [Flux Operator](https://fluxoperator.dev/)
- [Flux CD](https://fluxcd.io/)
- [Envoy Gateway](https://gateway.envoyproxy.io/)
- [Metrics Server](https://github.com/kubernetes-sigs/metrics-server)
- [Redis Operator](https://github.com/OT-CONTAINER-KIT/redis-operator)
- [cert-manager](https://cert-manager.io/)
- [kube-bench](https://github.com/aquasecurity/kube-bench)

## Requirements

Setup these tools before you start:

- [mise-en-place](https://mise.jdx.dev/)
- [kind](https://kind.sigs.k8s.io/)

## Quick Start

1. Clone the repository
1. Install dependencies:
   ```shell
   mise trust
   mise install
   ```
1. Configure repo access:
   ```shell
   # Example:
   # REPO_URL=ssh://user@my-computer/home/user/path/to/kind-nibbler
   mise set GIT_REPO_URL=$REPO_URL --file mise.local.toml
   ```
1. Start the `nibbler` cluster:
   ```shell
   mise task run install-cluster
   ```
1. Connect the repository:
   ```shell
   # Follow the outputs from the task
   mise task run connect-flux-repo
   ```
1. Discover the cluster:
   ```shell
   kubectl get all -A
   ```
1. (Optional) Apply custom configuration:
   ```shell
   # Edit files in ./custom-config and run:
   mise task run apply-custom-config
   ```
1. Uninstall the cluster:
   ```shell
   mise task run uninstall-cluster
   ```

## Workflow

1. Adjust config, add components, etc.
1. Commit it (no push required)
1. Let Flux apply it:
   ```shell
   flux reconcile ks flux-system --with-source
   ```

## Additional cluster

1. Create a new config in [kind](kind)
1. Add [cluster](clusters) configs
1. Let mise use your new configs
   ```shell
   mise set CLUSTER=zoidberg --file mise.local.toml
   ```
1. Run the steps from above
