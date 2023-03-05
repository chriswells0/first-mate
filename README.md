# First Mate

First Mate keeps watch over your Kubernetes "cargo" and automatically updates container images when new versions are released.

The chart is currently very simple: it does exactly what I needed (keeps images updated) and nothing more.
Major version updates are ignored since they would likely require configuration changes as well.
Please refer to the values.yaml file for configuration options.

Additional features such as sending email notifications when updates are applied may be added in the future.
Of course, contributions are welcome!

## Usage

[Helm](https://helm.sh) must be installed to use the chart.
Please refer to Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

  helm repo add first-mate https://chriswells0.github.io/first-mate

If you had already added this repo earlier, run `helm repo update` to retrieve the latest versions of the packages.
You can then run `helm search repo first-mate` to see the charts.

To install the first-mate chart:

    helm install my-first-mate first-mate/first-mate

To uninstall the chart:

    helm delete my-first-mate
