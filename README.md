# Azure Managed Prometheus - Tutorials

## What is Prometheus

https://prometheus.io/docs/tutorials/getting_started/

<img width="439" alt="image" src="https://github.com/user-attachments/assets/5d3f0efa-adcf-42b0-85c4-72af614486cf" />


## Setup Prometheus on a VM

1. Create a VM on the Azure portal.
2. Connect to the VM (you can either use SSH or remote-desktop).
3. Install Prometheus server: https://prometheus.io/docs/prometheus/latest/getting_started/, https://prometheus.io/download/
      - wget https://github.com/prometheus/prometheus/releases/download/v3.1.0/prometheus-3.1.0.linux-amd64.tar.gz
      - tar xvfz prometheus-*.tar.gz
      - cd prometheus-*
4. Locate the Prometheus config (prometheus.yml)
5. We now have the Prometheus server running, so we can configure the Prometheus to monitor itself. To do this, update the prometheus.yml config with the below:

```yaml
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'codelab-monitor'

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['localhost:9090']
```

6. Start Prometheus
By default, Prometheus stores its database in ./data (flag --storage.tsdb.path).<br>
./prometheus --config.file=prometheus.yml

7. Access the Prometheus server in VM-ip-address:9090

8. Query Prometheus metrics (eg. *up* or *process_cpu_seconds_total*)

9. Install Prometheus node-exporter to add endpoints for Prometheus server to scrape.

      - wget https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_exporter-1.8.2.linux-amd64.tar.gz
      - tar xvfz node_exporter-1.8.2.linux-amd64.tar.gz
      - cd node-exporter*

10. Run the node-exporter

	- ./node_exporter --web.listen-address VM-ip-address:8080

11. Configure prometheus (Prometheus.yml) and add the node-exporter target

     - Add the target in the target section:  - targets: ['VM-ip-address:8080']
   
12. Run Prometheus again, and view the metrics from node-exporter in the Prometheus UI.

## Setup Prometheus on a Kubernetes cluster

For this you can use any Kubernetes cluster. In this example, we will use an AKS cluster.

### Prerequisites

1. kubectl installed - kubectl enables you to manage/interact with the cluster. If you don't have kubectl, don't worry, we can use the Azure Cloud Shell

### Steps

1. Create an AKS cluster.
2. Connect to the AKS cluster using below commands on az cli:

```
az login
az account set --subscription subscription-id
az aks get-credentials --resource-group resourcegroupname --name aksclustername
```
3. Install Prom Operator:
 - helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
 - helm repo update
 - helm install main prometheus-community/kube-prometheus-stack
 - kubectl get all 
4. Now Prom and grafana and some default alerts are running
5. View grafana: Usually its port 3000 but you can check grafana pod logs to find the port.
   - Kubectl get pod
   - Kubectl logs grafana-pod-name
   - kubectl port-forward deployment/main-grafana 3000
   - Username: admin | password: prom-operator
6. View Prom UI: usually port 9090
	- kubectl port-forward prometheus-main-kube-prometheus-stack-prometheus-0 9090

