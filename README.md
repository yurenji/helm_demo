

### 1. First, install Minikube and start a local cluster:
```bash
minikube start --driver=docker
```
### 2. Enable the Minikube Docker daemon:
```bash
eval $(minikube docker-env)
```
### 3. Build the web application Docker image:
```bash
cd webapp
DOCKER_CONFIG=~/.docker/local docker build -t webapp:latest .
```
### 4. Add the PostgreSQL Helm repository and install PostgreSQL:
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-postgresql bitnami/postgresql \
  --set auth.postgresPassword=postgres \
  --set auth.database=mydatabase \
  --set auth.username=myuser \
  --set auth.password=mypassword
# Get passwords 
export POSTGRES_PASSWORD=$(kubectl get secret --namespace default my-postgresql -o jsonpath="{.data.password}" | base64 -d)
export POSTGRES_ADMIN_PASSWORD=$(kubectl get secret --namespace default my-postgresql -o jsonpath="{.data.postgres-password}" | base64 -d)
# port forward to the database
kubectl port-forward --namespace default svc/my-postgresql 5432:5432 
# connect to the database
PGPASSWORD="$POSTGRES_PASSWORD" psql --host 127.0.0.1 -U myuser -d mydatabase -p 5432
```
### 5. Install your web application chart:
```bash
cd ../k8s-webapp
helm install myapp .
```

### 6. Initialize the database:
```bash
minikube service myapp-webapp
```
### 7. Access the application:
```bash
minikube service myapp-webapp
```
This will open your default browser with the application running. You can now add notes, and they will be stored in the PostgreSQL database.
To clean up:
```bash
helm uninstall myapp
helm uninstall postgres
minikube stop
```

This demo shows:
- How to create a simple Helm chart
- How to use Helm dependencies (PostgreSQL)
- How to configure applications using Helm values
- How to deploy a web application that connects to a database
- Basic Kubernetes concepts (Deployments, Services)

The application is basic but demonstrates the core concepts of using Helm with Kubernetes. You can extend it by:
- Adding more features to the web application
- Implementing proper security measures
- Adding persistent volume claims for PostgreSQL
- Setting up ingress rules
- Adding health checks and readiness probes