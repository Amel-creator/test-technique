# HTTP Echo

## Description
Déploiement de l'application **HTTP Echo**, basée sur le service [mendhak/http-https-echo](https://hub.docker.com/r/mendhak/http-https-echo). Cette application permet de tester des requêtes HTTP et HTTPS en affichant les informations de chaque requête (headers, body, etc.).

---

## Prérequis
- Kubernetes (ex: Kind, Minikube)
- `kubectl` installé et configuré
- Ingress Controller NGINX installé et fonctionnel
- `curl` ou un navigateur pour tester les requêtes

---

## Structure du projet
```php
http-https-echo/
├── deployment.yaml      # Déploiement des pods
├── service.yaml         # Service ClusterIP pour exposer les pods
├── ingress.yaml         # Ingress pour accéder via un hostname
└── README.md
```

---

## Déploiement

### 1. Appliquer les manifests Kubernetes
```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml
```

### 2. Vérifier que les pods et services sont en état Running :
```bash
kubectl get all -n default
kubectl get ingress -n default
```

---

## Ports et configuration

- Container HTTP : 8080
- Container HTTPS : 8443

Nous utilisons le port 8080 pour HTTP pour éviter les conflits avec le port 80 sur l’hôte.

- Service : http-https-echo-svc
    - Type : ClusterIP
    - Target Port : 8080

- Ingress : echo.local

---

## Tester l'application

### Option 1 : Port-forward : 

```bash
kubectl port-forward svc/http-https-echo-svc 8080:8080
```
Puis dans un autre terminal : 

```bash
curl http://localhost:8080/
```

### Option 2 : Via l'INgress : 

Ajouter la ligne suivante dans /etc/hosts :

```bash
127.0.0.1 echo.local
```
Puis accéder depuis le navigateur ou curl :

```bash
curl http://echo.local/
```

---

## Notes

- Tous les pods utilisent l’image mendhak/http-https-echo:31 comme recommandé;

- Les modifications de ports (80 → 8080) ont été faites pour éviter les conflits sur l’hôte.

- Pour HTTPS, le port est exposé en interne à 8443. Pour l’activer, configurer les certificats dans le deployment.
