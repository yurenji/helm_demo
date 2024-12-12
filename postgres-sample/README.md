## Install Postgres in k8s cluster without helm

```bash
kubectl apply -f pv.yaml  
kubectl apply -f pvc.yaml 
kubectl apply -f configmap.yaml 
kubectl apply -f secret.yaml 
kubectl apply -f deployment.yaml 
kubectl apply -f service.yaml 

```

## With Helm 
```bash
helm install my-postgres bitnami/postgresql --set auth.username=myuser --set auth.password=mypassword --set auth.database=mydb
```
