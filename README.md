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

### Тестируем разрешенные соединения
<img width="1160" height="550" alt="image" src="https://github.com/user-attachments/assets/edfabcb0-d0ec-488a-846b-dda3e0a2724d" />

<img width="1143" height="498" alt="image" src="https://github.com/user-attachments/assets/8ff8fcd2-5453-4499-9d82-fd4bdcc164b0" />


### Тестируем запрещенные соединения

<img width="1183" height="160" alt="image" src="https://github.com/user-attachments/assets/b9fd68fa-156b-4cb1-9504-dc7fcec0abfe" />

<img width="1169" height="154" alt="image" src="https://github.com/user-attachments/assets/dd945c90-e645-412f-b5ee-f3e1607c75c5" />


