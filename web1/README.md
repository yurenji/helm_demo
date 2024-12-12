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
helm install mytime ./helm/rj_web1
helm install mytime2 ./helm/rj_web1 --set service.nodePort=30002
```

3. Access the application:
```bash
minikube service mytime-service
minikube service mytime2-service
```
The application will open in your default browser, showing the current time that updates every second.

To uninstall the application:
```bash
helm uninstall mytime
helm uninstall mytime2
```
