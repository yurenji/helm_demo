1. Build the Docker image:
```bash
# minikube start --driver=docker
# eval $(minikube docker-env)
# brew install docker-buildx
# docker buildx create --use
docker build -t rj_web1:latest .

# No need to run below when building directly in Minikube
# minikube image load rj_web1:latest
```

2. Install the Helm chart:
```bash
helm install rj_web1 .
```

3. Access the application:
```bash
minikube service rj_web1
```
The application will open in your default browser, showing the current time that updates every second.

To uninstall the application:
```bash
helm uninstall rj_web1
```
