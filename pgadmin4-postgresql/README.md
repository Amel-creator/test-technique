# pgAdmin4 avec PostgreSQL

## Description
Déploiement de **pgAdmin4** connecté à une base de données **PostgreSQL** sur Kubernetes (ex: Minikube).

---

## Prérequis
- `kubectl` installé et connecté à ton cluster (ex: Minikube)
- Minikube ou un cluster Kubernetes local en cours d’exécution
- Structure du projet :

```php
├── postgres/
│   ├── secret.yaml
│   ├── pvc.yaml
│   ├── deployment.yaml
│   └── service.yaml
└── pgadmin/
├── deployment.yaml
├── service.yaml
└── ingress.yaml
```

---

## Étapes de déploiement

### 1. Nettoyer les anciens déploiements
```bash
kubectl delete all --all
kubectl delete pvc --all
kubectl delete secret --all
kubectl delete configmap --all
```

### 2. Créer les secrets et volumes

```bash
kubectl apply -f postgres/secret.yaml
kubectl apply -f postgres/pvc.yaml
```

### 3. Déployer PostgreSQL

```bash
kubectl apply -f postgres/deployment.yaml
kubectl apply -f postgres/service.yaml
```

Vérifier que le pod est en cours d’exécution :

```bash
kubectl get pods -l app=postgres
```

### 4. Déployer pgAdmin

```bash
kubectl apply -f pgadmin/deployment.yaml
kubectl apply -f pgadmin/service.yaml
kubectl apply -f pgadmin/ingress.yaml
```

Vérifier le déploiement :

```bash
kubectl get pods -l app=pgadmin
kubectl get svc
```

## Accéder à pgadmin

### Option 1 — Port-forward

```bash
kubectl port-forward svc/pgadmin-svc 8080:80
```

Ouvrir dans un navigateur :

*http://localhost:8080*

### Option 2 — Ingress + DNS local

Ajouter dans /etc/hosts :

```bash
127.0.0.1 pgadmin.local
```

Ouvrir dans un navigateur :

*http://pgadmin.local*


## Identifiants de connexion pgAdmin

- Email : admin@example.com
- Mot de passe : admin