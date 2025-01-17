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

	3. Install Prom Operator:
		a. helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
		b. Helm repo update
		c. helm install main prometheus-community/kube-prometheus-stack
		d. Kubectl get all 
	4. Now Prom and grafana and some default alerts are running
	5. View grafana: Usually its port 3000 but you can check grafana pod logs to find the port.
		a. Kubectl get pod
		b. Kubectl logs <grafana pod name>
		c. kubectl port-forward deployment/main-grafana 3000
		d. Username: admin | password: prom-operator
	6. View Prom UI: usually port 9090
kubectl port-forward prometheus-main-kube-prometheus-stack-prometheus-0 9090![image](https://github.com/user-attachments/assets/87c63e13-28f8-47ff-8e44-dc93e05176b5)

