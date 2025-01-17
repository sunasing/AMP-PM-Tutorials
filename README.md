# Azure Managed Prometheus - Tutorials

## Setup Prometheus on a VM

1. Create a VM on the Azure portal.
2. Connect to the VM (you can either use SSH or remote-desktop).
3. Install Prometheus server: https://prometheus.io/docs/prometheus/latest/getting_started/
      tar xvfz prometheus-*.tar.gz
      cd prometheus-*
4. Install Prometheus node-exporter: https://prometheus.io/download/

5. Start Prometheus.
By default, Prometheus stores its database in ./data (flag --storage.tsdb.path).
./prometheus --config.file=prometheus.yml


## Setup Prometheus on a Kubernetes cluster

For this you can use any Kubernetes cluster. In this example, we will use an AKS cluster.
