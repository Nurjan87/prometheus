# kubernetes-prometheus
Configuration files for setting up prometheus monitoring on Kubernetes cluster.

also you can clone this repository :
 git clone https://github.com/Nurjan87/prometheus.git

# First, we will create a Kubernetes namespace for all our monitoring components. Execute the following command to create a new namespace called monitoring.
kubectl create namespace monitoring

# Create the role using the following command.
kubectl create -f clusterRole.yaml

# Execute the following command to create the config map in Kubernetes.
kubectl create -f config-map.yaml

# kubectl create  -f prometheus-deployment.yaml
kubectl create  -f prometheus-deployment.yaml 

# You can check the created deployment using the following command.
kubectl get deployments --namespace=monitoring

# First, get the Prometheus pod name.
kubectl get pods --namespace=monitoring

# Execute the following command with your pod name to access Prometheus from localhost port 8080.
#       Note: Replace prometheus-monitoring-3331088907-hm5n1 with your pod name.
kubectl port-forward prometheus-deployment-56f854c695-7ffct 8080:9090 -n monitoring

kubectl expose pod prometheus-deployment-56f854c695-7ffct --type=NodePort Port 8080:9090 -n monitoring

# Create the service using the following command.
kubectl create -f prometheus-service.yaml --namespace=monitoring

# How to get service 
kubectl get service -n monitoring

kubectl get svc -o wide -n monitoring

 kubectl get nodes  -o wide     //get external-ip

kubectl get pods --all-namespaces


# Once created, you can access the Prometheus dashboard using any Kubernetes node IP on port 30000.

# Now if you browse to status --> Targets, you will see all the Kubernetes endpoints connected to Prometheus automatically using service discovery as shown below. So you will get all kubernetes container and node metrics in Prometheus.
  



#  FOR GRAFANA :

# This tutorial explains the Grafana setup on a Kubernetes cluster. You can create dashboards on Grafana for all the Kubernetes metrics through prometheus.

# Lets get started with the setup.

# Step 1: Create file named grafana-datasource-config.yaml

vi grafana-datasource-config.yaml

# Note: The following datasource configuration is for prometheus. If you have more data sources, you can add more data sources with different YAMLs under data section.

# Step 2: Create the configmap using the following command.

kubectl create -f grafana-datasource-config.yaml

# Step 3: Create a file named deployment.yaml

vi grafan-deployment.yaml

# step 4: Create the deployment

kubectl create -f deployment.yaml


# Step 5: Create a service file named service.yaml

vi grafan-service.yaml

# Step 6: Create the service.

kubectl create -f service.yaml


# Now you should be able to access the Grafana dashboard using any node IP on port 32000. Use the following default username and password to login. Once you login with default credentials, it will prompt to change the default password.

User: admin
Pass: admin


