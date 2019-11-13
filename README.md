# final-jhipster

JHipster Application with 4 microservices

## Setup

### Connect to Project

```gcloud config set project payment-platform-204588```

### Create GKE Cluster

```
gcloud container clusters create final-cluster  --zone us-central1-a --num-nodes 4 --machine-type n1-standard-2
gcloud container clusters get-credentials final-cluster --zone us-central1-a
```

### Configure Kubernetes Cluseter Engine

```
kubectl create clusterrolebinding cluster-admin-binding --clusterrole=cluster-admin --user="shagunbandi@gmail.com"
```

### Install Istio and Helm on your local machine

##### 1. Install Helm ( https://github.com/helm/helm ) 

###### Run this script to install helm

```
https://raw.githubusercontent.com/helm/helm/master/scripts/get
```

##### 2. Install Istio

```
wget https://github.com/istio/istio/releases/download/1.0.6/istio-1.0.6-linux.tar.gz
tar xvfz istio-1.0.6-linux.tar.gz 
cd istio-1.0.6
kubectl apply -f install/kubernetes/helm/istio/templates/crds.yaml
kubectl apply -f install/kubernetes/istio-demo.yaml --as=admin --as-group=system:masters
```

###### Get Istio Ingress IP
		
```
kubectl get services -n istio-system ( 34.67.38.135 )
```

### Install Kiali

```
kubectl apply -f configuration.yaml (Find it in the enclosed folder)
```

### Install Elastic GKE Logging

```
GCP -> Kubernetes Engine -> Application -> Elastic GKE logging -> Create New Namespace -> Create
Change the ip to your ip in this file
kubectl apply -f kibana-gateway.yml
```

### Install Node 

```
curl -sL https://deb.nodesource.com/setup_13.x | sudo -E bash -
sudo apt-get install nodejs
```

### Install Jhipster

```
npm install -g generator-jhipster
npm install -g yo ( Optional )
```

### Create Jhipster Application from jdl file

```
script.jh ( Find it in the enclosed folder )	
```

### Build and Publish Doocker images
	
##### Run the ./mvnw command in each folder

```
./mvnw -ntp -Pprod  verify jib:build -Djib.to.image=shagunbandi/<Image Name>
```

##### example 

```
cd /home/shagunbandi/project/ui && ./mvnw -ntp -Pprod verify jib:build -Djib.to.image=shagunbandi/ui && cd /home/shagunbandi/project/organization && ./mvnw -ntp -Pprod verify jib:build -Djib.to.image=shagunbandi/organization && cd /home/shagunbandi/project/leave && ./mvnw -ntp -Pprod verify jib:build -Djib.to.image=shagunbandi/leave && cd /home/shagunbandi/project/meeting && ./mvnw -ntp -Pprod verify jib:build -Djib.to.image=shagunbandi/meeting && cd /home/shagunbandi/project/notification && ./mvnw -ntp -Pprod verify jib:build -Djib.to.image=shagunbandi/notification 
```

### Run Images on Kubernetes

```
gcloud container clusters get-credentials final-cluster --zone us-central1-a
cd kubernetes
kubectl apply -f namespace.yml
kubectl label namespace avengers istio-injection=enabled --overwrite=true
kubectl apply -f ui/	
kubectl apply -f organization/
kubectl apply -f leave/
kubectl apply -f meeting/
kubectl apply -f notification/
kubectl apply -f istio/
```

### Check Deployment Status

```
kubectl get pods -n avengers
```

##### If everything is Running try http://ui.avengers.34.67.38.135.nip.io


