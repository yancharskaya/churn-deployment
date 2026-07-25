# Churn Deployment
•	Порядок работы: выбрать кластер и поднять его → Helm руками → Argo CD → автоматизация. 
•	Статус: связка «код → образ → живой кластер».

## Установка k3d
curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash

### Создание кластера
k3d cluster create dev --agents 1 -p "8080:80@loadbalancer"

## Установка helm
https://helm.sh/docs/intro/install/

## Установка Argo CD
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

### Проверка pods Argo CD
kubectl get pods -n argocd

### Пароль администратора (логин: admin)
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath='{.data.password}' | base64 -d ; echo

### Открыть интерфейс: пробрасываем порт из кластера себе
kubectl port-forward svc/argocd-server -n argocd 8080:443
-> открыть https://localhost:8080 (браузер поругается на сертификат — это нормально, «всё равно перейти»)

### Добавление монтиоринга репозитория с деплойментами
kubectl apply -f k8s/argocd-application.yaml
в UI появится карточка churn-service; через несколько секунд:
Sync Status: Synced      (кластер совпадает с git)
Health:      Healthy     (поды живы и готовы)
kubectl get pods  - ваши поды подняты Argo CD
