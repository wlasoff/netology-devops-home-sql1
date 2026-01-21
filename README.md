# Домашнее задание к занятию "Как работает сеть в K8s" - `Молоствов Андрей`


### Задание 

```
kubectl create namespace app
nano frontend.yaml
nano backend.yaml
nano cache.yaml
kubectl apply -f frontend.yaml
kubectl apply -f backend.yaml
kubectl apply -f cache.yaml
kubectl expose deployment frontend -n app --port=80 --target-port=80
kubectl expose deployment backend -n app --port=80 --target-port=80
kubectl expose deployment cache -n app --port=80 --target-port=80
nano default-deny.yaml
nano frontend-to-backend.yaml
nano backend-to-cache.yaml
nano allow-dns.yaml
kubectl apply -f default-deny.yaml
kubectl apply -f frontend-to-backend.yaml
kubectl apply -f backend-to-cache.yaml
kubectl apply -f allow-dns.yaml
FRONTEND_POD=$(kubectl get pods -n app -l app=frontend -o jsonpath='{.items[0].metadata.name}')
BACKEND_POD=$(kubectl get pods -n app -l app=backend -o jsonpath='{.items[0].metadata.name}')
CACHE_POD=$(kubectl get pods -n app -l app=cache -o jsonpath='{.items[0].metadata.name}')
nano frontend-egress.yaml
nano backend-egress.yaml
kubectl apply -f frontend-egress.yaml
kubectl apply -f backend-egress.yaml
kubectl exec -n app $FRONTEND_POD -- curl -v http://backend.app.svc.cluster.local
kubectl exec -n app $BACKEND_POD -- curl -v http://cache.app.svc.cluster.local
kubectl exec -n app $BACKEND_POD -- timeout 3 curl -v http://frontend.app.svc.cluster.local
kubectl exec -n app $FRONTEND_POD -- timeout 3 curl -v http://cache.app.svc.cluster.local
```

### Изменил порт на 8080

<img width="901" height="200" alt="image" src="https://github.com/user-attachments/assets/5c198b14-34b6-466b-a04f-a4c92560b222" />

