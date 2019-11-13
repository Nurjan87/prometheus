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


1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana
  namespace: monitoring
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      name: grafana
      labels:
        app: grafana
    spec:
      containers:
      - name: grafana
        image: grafana/grafana:latest
        ports:
        - name: grafana
          containerPort: 3000
        resources:
          limits:
            memory: "2Gi"
            cpu: "1000m"
          requests: 
            memory: "1Gi"
            cpu: "500m"
        volumeMounts:
          - mountPath: /var/lib/grafana
            name: grafana-storage
          - mountPath: /etc/grafana/provisioning/datasources
            name: grafana-datasources
            readOnly: false
      volumes:
        - name: grafana-storage
          emptyDir: {}
        - name: grafana-datasources
          configMap:
              defaultMode: 420
              name: grafana-datasources
Step 4: Create the deployment

kubectl create -f deployment.yaml
1
kubectl create -f deployment.yaml
Step 5: Create a service file named service.yaml

vi service.yaml
1
vi service.yaml
Copy the following contents. This will expose Grafana on NodePort 32000. You can also expose it using ingress or a Loadbalancer based on your requirement.

apiVersion: v1
kind: Service
metadata:
  name: grafana
  namespace: monitoring
  annotations:
      prometheus.io/scrape: 'true'
      prometheus.io/port:   '3000'
spec:
  selector: 
    app: grafana
  type: NodePort  
  ports:
    - port: 3000
      targetPort: 3000
      nodePort: 32000
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
apiVersion: v1
kind: Service
metadata:
  name: grafana
  namespace: monitoring
  annotations:
      prometheus.io/scrape: 'true'
      prometheus.io/port:   '3000'
spec:
  selector: 
    app: grafana
  type: NodePort  
  ports:
    - port: 3000
      targetPort: 3000
      nodePort: 32000
Step 6: Create the service.

kubectl create -f service.yaml
1
kubectl create -f service.yaml
Now you should be able to access the Grafana dashboard using any node IP on port 3200. Use the following default username and password to login. Once you login with default credentials, it will prompt to change the default password.

User: admin
Pass: admin
1
2
User: admin
Pass: admin
Setup Kubernetes Dashbaords
There are many prebuilt Grafana templates available for various data sources. You can check out the templates from here.

Setting up a dashboard from a template is pretty easy. Follow the steps given below to setup a dashboard to monitor kubernetes deployments.

Step 1: Get the template ID from grafana public template. as shown below.


Step 2: Head over to grafana and select the import option.


Step 3: Enter the dashboard ID you got it step 1


Step 4: Grafana will automatically fetch the template from Grafana website. You can change the values as shown in the image below and click import.


You should see the dashboard immediately.


Conclusion
Grafana is a very powerful tool when it comes to dashboards. It is used by many organisations to monitor its workloads. Let us know how you are using Grafana in your organisation. Also let us know if you want to add more information to this article.
