# Explaining Prometheus is out of the scope of this article. In this article, I will guide you to setup Prometheus on a Kubernetes cluster and collect node, pods and services metrics automatically using Kubernetes service discovery configurations

# kubernetes-prometheus comands 
git clone https://github.com/Nurjan87/prometheus.git     //this my git repository git clone it

# First, we will create a Kubernetes namespace for all our monitoring components. Execute the following command to create a new namespace called monitoring.

kubectl create namespace monitoring  

# Create the role using the following command

kubectl create -f clusterRole.yaml 

# Execute the following command to create the config map in Kubernetes.

kubectl create -f config-map.yaml

# Create a deployment on monitoring namespace using the above file

kubectl create  -f prometheus-deployment.yaml 

# You can check the created deployment using the following command.

kubectl get deployments --namespace=monitoring

# First, get the Prometheus pod name

kubectl get pods --namespace=monitoring

# Execute the following command with your pod name to access Prometheus from localhost port 8080

kubectl port-forward prometheus-deployment-56f854c695-7ffct 8080:9090 -n monitoring

kubectl expose pod prometheus-deployment-56f854c695-7ffct --type=NodePort Port 8080:9090 -n monitoring

# Create the service using the following comman

kubectl get service -n monitoring

kubectl get svc -o wide -n monitoring

 kubectl get node  -o wide 

 

