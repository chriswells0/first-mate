# First Mate

First Mate monitors your Kubernetes "cargo" and automatically updates container images when new versions are released.

This chart is currently very simple: it does exactly what was needed (keeps images updated) and nothing more.
It creates a CronJob that runs every hour, which then (when using the default configuration):

* Finds all DaemonSets, Deployments, and StatefulSets across all namespaces except kube-node-lease, kube-public, kube-system, and kubernetes-dashboard.
* Retrieves the latest tags for each referenced image.
* If a newer version is found, the DaemonSet/Deployment/StatefulSet is updated to use the newer tag.

Major version updates are ignored since they're more likely to require application/configuration changes (human intervention).

Additional features such as sending email notifications when updates are applied may be added in the future.
Of course, contributions are welcome!

## Configuration

Please refer to the chart's `values.yaml` file for a full list of configuration options.
Some commonly used inputs are below.

| Name                      | Description                                                                 | Default Value           |
| ------------------------- | --------------------------------------------------------------------------- | ------------------------- |
| `controllerKinds`         | Kinds of controllers whose images should be updated                         | `["daemonsets", "deployments", "statefulsets"]`            |
| `ignoredVersions`         | Versions to skipp even if they're newer                                     | `["latest"]` (`latest` cannot be compared as a version)           |
| `namespaces.ignored`      | Namespaces whose images should NOT be updated                               | `["kube-node-lease", "kube-public", "kube-system", "kubernetes-dashboard"]`            |
| `namespaces.monitored`    | Specific namespaces to monitor for updates                                  | `null` (monitors all namespaces not in the `ignored` list)            |
| `schedule`                | Interval at which the CronJob runs                                          | `"0 * * * *"`   |

First Mate can be installed as a single instance per cluster and given access to all namespaces OR it can be installed into each namespace being monitored.
It's recommended to install once per cluster, but that requires cluster-level privileges and may not be allowed in some environments.

When running at the cluster level, use `namespaces.ignored` and `namespaces.monitored` to control the scope of monitoring.
An empty/omitted `namespaces.monitored` list means "all namespaces that aren't in the `namespaces.ignored` list."
If a namespace is in both `namespaces.ignored` and `namespaces.monitored`, it will be ignored.

To use per-namespace instances of this chart, set `namespaces` to a "falsey" value (e.g., `null` or an empty value).
This prevents the ClusterRole and ClusterRoleBinding from being created, and only the chart's release namespace will be monitored for updates.

Example:

```
namespaces: null
```

## Usage

[Helm](https://helm.sh) must be installed to use the chart.
Please refer to Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

    helm repo add first-mate https://chriswells0.github.io/first-mate

If you had already added this repo earlier, run `helm repo update` to retrieve the latest versions of the packages.
You can then run `helm search repo first-mate` to see the charts.

To install the first-mate chart with the release name `my-first-mate`:

    helm install my-first-mate first-mate/first-mate

To uninstall the chart:

    helm delete my-first-mate
