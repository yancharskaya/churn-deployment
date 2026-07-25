# Churn Deployment
•	Порядок работы: выбрать кластер и поднять его → Helm руками → Argo CD → автоматизация. 
•	Статус: связка «код → образ → живой кластер».

### Установка k3d
curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash

### Установка helm
https://helm.sh/docs/intro/install/