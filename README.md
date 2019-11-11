# kubernetes-prometheus
Configuration files for setting up prometheus monitoring on Kubernetes cluster.

kubectl create namespace monitoring

kubectl create -f clusterRole.yaml

kubectl create -f config-map.yaml

kubectl create  -f prometheus-deployment.yaml 

kubectl get deployments --namespace=monitoring

kubectl get pods --namespace=monitoring

kubectl port-forward prometheus-deployment-56f854c695-7ffct 8080:9090 -n monitoring

kubectl expose pod prometheus-deployment-56f854c695-7ffct --type=NodePort Port 8080:9090 -n monitoring

kubectl get service -n monitoring

kubectl get svc -o wide -n monitoring

 kubectl get node  -o wide 

 

