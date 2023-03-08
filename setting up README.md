# Setting-up-Prometheus-Grafana


Prerequisite: 
 
1)	Kubernetes Cluster With StorageClass Configured (Dynamic Provisioning). 
 
If Storage Class is not Configured then make persistence enabled as false in helm
values files 	# Which is not suggestable as if pods are terminated and recreated we 
will lost the data. 
 
2)	Kubernetes nodes with minimum 4GB RAM With 2 Core Processer. 
 
3)	Server which has kubectl & Helm Configured. 
 
 
Deploy Prometheus, Exporters, Alert Manager & Grafana 
https://github.com/myLandmakTechnology/prometheus-grafana-ELK-EFK 
 
1)	Add helm repo which contains Prometheus and Grafana charts. 
 
 
helm repo add stable  "https://charts.helm.sh/stable" 
 
2)	Create a namespace in which will deploy Prometheus & it’s components. 
kubectl create ns monitoring 
 
3)	Get prometheus values 
 helm show values stable/prometheus >> prometheusvalues.yml 
 
4)	Update prometheusvalues.yml file to update service types and add alert rules & SMTP details. 
 
 
5)	Deploy using helm with updated values file. 
 
helm install -f Prometheus.values.yml prometheus stable/prometheus -n monitoring 
 
helm install prometheus stable/prometheus -n monitoring -f Prometheus.values.yml 
 
 
 
6)	Get Grafana values file. 
 helm inspect values stable/grafana >> grafanavalues.yml 
 
7)	Update grafanavalues.yml to update service type. 
 
 
 
8)	Deploy Grafana using Helm with updated values file. 
 helm install -f Grafana-values.yml grafana stable/grafana -n monitoring 
 
9)	Access Grafana Using NodeIP & NodePort If Service type is Node Port or accessing using Load Balancer if service type is load balancer. 
 
 
10)	Get Grafana default password to login. Default user name is admin. 
 
kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo 
11)	Configure Data Source Service URL(Below URL) in Grafana so that it use this data source to fetch metrics. 
 http://prometheus-server 
 
12)	import some sample dashboards from community find few ids below. 
 
Sample Dash Board IDS: 3119,7249 8919,6417 ,11074. 
