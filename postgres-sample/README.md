

### 1. First, install Minikube and start a local cluster:
```bash
minikube start --driver=docker
```
### 2. Enable the Minikube Docker daemon:
```bash
eval $(minikube docker-env)
```
### 3. Install Postgres in K8s cluster

#### 3.1 With Helm
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-postgresql bitnami/postgresql \
  --set auth.postgresPassword=postgres \
  --set auth.database=mydatabase \
  --set auth.username=myuser \
  --set auth.password=mypassword
```

connect to the database
```bash
# Get passwords 
export POSTGRES_PASSWORD=$(kubectl get secret --namespace default my-postgresql -o jsonpath="{.data.password}" | base64 -d)
export POSTGRES_ADMIN_PASSWORD=$(kubectl get secret --namespace default my-postgresql -o jsonpath="{.data.postgres-password}" | base64 -d)
# port forward to the database
kubectl port-forward --namespace default svc/my-postgresql 5432:5432 
# connect to the database
PGPASSWORD="$POSTGRES_PASSWORD" psql --host 127.0.0.1 -U myuser -d mydatabase -p 5432
```

#### 3.2 Without Helm

```bash
kubectl apply -f pv.yaml  
kubectl apply -f pvc.yaml 
kubectl apply -f configmap.yaml 
kubectl apply -f secret.yaml 
kubectl apply -f deployment.yaml 
kubectl apply -f service.yaml 
```
