Ubuntu (WSL)

sudo apt update -y

_________________________________________________________________________________

 Install Docker (inside WSL)

sudo apt install docker.io -y

Start Docker:

sudo service docker start

Give permission:

sudo usermod -aG docker $USER

Close & reopen Ubuntu terminal
Verify:

docker ps

-------------------------------------------------------------------------------------------------------------------

 Install kubectl

curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl


chmod +x kubectl


sudo mv kubectl /usr/local/bin/


kubectl is the primary command-line tool for interacting with a Kubernetes cluster
Check:

kubectl version --client




---------------------------------------------------------------------------------------------------
 Install Minikube (Kubernetes Cluster)

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube

Start Kubernetes:

minikube start --driver=docker

Verify:

kubectl get nodes


 Explain:
Minikube creates a local Kubernetes cluster.
Minikube is a lightweight, single-node Kubernetes cluster tool for local development





Helm is the package manager for Kubernetes, simplifying the definition, installation, and upgrading of even complex applications by bundling all necessary Kubernetes resources into single, reusable packages called Helm Charts.



 It acts like a software installer for Kubernetes, letting you manage applications with commands like install, upgrade, and rollback, reducing manual configuration and ensuring consistency across different environments

---------------------------------------------------------------------------------------------------
 
Install Helm (Package Manager)

curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash


Check:

helm version
________________________________________________________________________

 Deploy Sample Application (nginx)

kubectl create deployment nginx --image=nginx

kubectl expose deployment nginx --type=NodePort --port=80

Check:

kubectl get pods

kubectl get svc


Access app:

minikube service nginx

Kubernetes is now running our application inside a Pod.
---------------------------------------------------------------------------------------------------


Install Prometheus (Monitoring)

Add Helm repo:

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm repo update

Install Prometheus:

helm install prometheus prometheus-community/prometheus

Check:

kubectl get pods

Port forward Prometheus:

kubectl port-forward svc/prometheus-server 9090:80

Open browser:

http://localhost:9090


Test query:

up



 Explain:
Prometheus is scraping Kubernetes metrics automatically.
----------------------------------------------------------------------------------------------------


Install Grafana (Visualization)

helm repo add grafana https://grafana.github.io/helm-charts

helm repo update

helm install grafana grafana/grafana


Get password:

kubectl get secret grafana -o jsonpath="{.data.admin-password}" | base64 --decode && echo


Port forward:

kubectl port-forward svc/grafana 3000:80


Open:

http://localhost:3000


Login:

•	Username: admin

•	Password: (from above)
_____________________________________________________________________________________


  Connect Prometheus to Grafana

Grafana UI:

1.	 Settings → Data Sources

2.	Add Prometheus

3.	URL:
http://prometheus-server.monitoring.svc.cluster.local

4.	Save & Test 

_____________________________________________________________________________________


Import Kubernetes Dashboard 

Grafana → Dashboards → Import

Use Dashboard ID:
6417 (or) 3662

 You will see:

•	CPU usage

•	Memory usage

•	Pod count

•	Node health

