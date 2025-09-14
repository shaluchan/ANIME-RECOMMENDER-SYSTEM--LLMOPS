# Anime Recommender System- LLMOPS
I built an LLMOps-based Anime Recommender System that uses Large Language Models and sentence embeddings to recommend anime based on user descriptions. The project is fully containerized with Docker, deployed on Kubernetes with secure secret management, and monitored using Grafana. It is hosted on AWS, showcasing my ability to design and manage cloud-native, scalable, and production-ready systems while practicing end-to-end MLOps/LLMOps workflows with a strong focus on DevOps skills.
![Anime](/images/anime.png)
## 🚀 Features

#### LLM-powered Recommendations  
- Uses **Groq’s LLaMA-3.1-8B-Instant** for natural language understanding.  
- Uses **Hugging Face’s sentence-transformers/all-MiniLM-L6-v2** for semantic similarity search.

#### Interactive UI with Streamlit  
- Simple and user-friendly **web interface** for anime recommendations.  

#### Data-driven  
- Recommends anime based on a **preprocessed dataset** and **semantic matching**.  

#### Containerized with Docker  
- Ensures **reproducible** and **portable** deployment.  

#### Kubernetes Deployment  
- **Secrets injected** directly into the Kubernetes cluster.  
- Hosted on an **AWS EC2 t2.medium instance**.  

#### Monitoring with Grafana  
- Provides **real-time performance** and **resource monitoring**.  

## 🛠️ Tech Stack

- **LLMs**:Groq (llama-3.1-8b-instant), Hugging Face (all-MiniLM-L6-v2)
- **Frontend/UI**: Streamlit  
- **Containerization**: Docker  
- **Orchestration**: Kubernetes (K8s)  
- **Infrastructure**: AWS EC2 (t2.medium)  
- **Monitoring**: Grafana
- **Secrets Management**: Kubernetes Secrets

## Project Working

### 1.Create a VM 
Create an instance on any cloud platform and add port 8501 in it's inbound firewall rules.Configure github on it, install docker,minikube and kubectl.
SSH into the instance.

### 2.Clone the Repo
```
git clone https://github.com/shaluchan/ANIME-RECOMMENDER-SYSTEM--LLMOPS.git
cd ANIME-RECOMMENDER-LLMOPS
```
### 3.Build the Image
```
# point Dcoker to Minikube
eval $(minikube docker-env)

docker build -t llmops-app:latest .
```
### 4.Inject Secrets inside the cluster
```
kubectl create secret generic llmops-secrets \
  --from-literal=GROQ_API_KEY="" \
  --from-literal=HUGGINGFACEHUB_API_TOKEN=""
```
### 5.Apply Deployment
```
kubectl apply -f llmops-k8s.yaml

# see pods running
kubectl get pods

```
### 6.Access the Application

Run the following command in two seperate terminals

1.Expose load balancer to your local network
```
minikube tunnel
```
2. Port-forward the service
```
kubectl port-forward svc/llmops-service 8501:80 --address 0.0.0.0
```

Now copy external ip and :8501 on your browser and see the app running

### 7.GRAFANA CLOUD MONITORING

```
# 1 open another vm terminal for grafana cloud and create a new namespace.

kubectl create ns monitoring

# 2 Install HELM on the vm.
# 3 Set up grafana.
# 4 create values.yaml by using the helm charts generated.

# 5 Install Grafana's Kuberneted MOnitoring stack using Helm

helm repo add grafana https://grafana.github.io/helm-charts &&
  helm repo update &&
  helm upgrade --install --atomic --timeout 300s grafana-k8s-monitoring grafana/k8s-monitoring \
    --namespace "monitoring" --create-namespace --values values.yaml

# 6 Go to grafana cloud,refresh it and You can see metrics related to your kubernetes cluster.

```
![Grafana-Monitoring](/images/grafana.png)
### 8. Clean up
Terminate your instance.

## ARCHITECTURAL DIAGRAM
![hld](/images/architecture.png)








