# Kubernetes Nginx Deployment Demo

Dieses Projekt demonstriert ein vollständiges Kubernetes-Deployment einer stateless Nginx-Anwendung. Dabei werden grundlegende Konzepte wie Deployment, Service, Rolling Update und Rollback praktisch angewendet.

## Projektstruktur

```
.
├── nginx-app-v1/          # Erste Nginx-Version (blau)
├── nginx-app-v2/          # Zweite Nginx-Version (grün)
├── kubernetes/            # YAML-Dateien für Deployment & Service
├── screenshots/           # Nachweise für Version 1, 2 & Rollback
├── reflexion.md          # Beantwortung der Aufgabenfragen
└── README.md             # Diese Datei
```

## Inhalt

- Zwei HTML-Versionen der App (blau & grün)
- Docker-Images auf Docker Hub veröffentlicht:
  - `vinjust/nginx-example:v1`
  - `vinjust/nginx-example:v2`
- Deployment von Version 1
- Rolling Update zu Version 2
- Rollback zurück auf Version 1
- Service mit NodePort
- Screenshots aller Phasen
- Reflexion zu den wichtigsten Konzepten

## Ausführen

### 1. Cluster starten
Starten Sie Ihren Kubernetes-Cluster (z.B. Minikube oder Docker Desktop)

### 2. Deployment & Service erstellen
```bash
kubectl apply -f kubernetes/nginx-deployment.yaml
kubectl apply -f kubernetes/nginx-service.yaml
```

### 3. Zugriff auf die App
- Port anzeigen mit: `kubectl get service nginx-app-service`
- Im Browser öffnen: `http://localhost:<NodePort>`

### 4. Rolling Update ausführen
- `nginx-deployment.yaml` auf `v2` ändern
- Dann:
```bash
kubectl apply -f kubernetes/nginx-deployment.yaml
kubectl rollout status deployment/nginx-app-deployment
```

### 5. Rollback durchführen
```bash
kubectl rollout undo deployment/nginx-app-deployment
```

## Screenshots

- `v1.png`: Version 1 (blau) im Browser
- `v2.png`: Version 2 (grün) nach Rolling Update
- `rollback.png`: Rückkehr zu Version 1

## Reflexion

Die Fragen zur Aufgabe wurden in `reflexion.md` beantwortet.

## Technische Details

### Kubernetes Konfiguration

#### Deployment (nginx-deployment.yaml)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-app-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-app
  template:
    metadata:
      labels:
        app: nginx-app
    spec:
      containers:
      - name: nginx-app
        image: vinjust/nginx-example:v1
        ports:
        - containerPort: 80
```

#### Service (nginx-service.yaml)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-app-service
spec:
  selector:
    app: nginx-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: (wird automatisch vergeben)
  type: NodePort
```

### Docker Images

#### Version 1 (Blau)
```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/
EXPOSE 80
```

#### Version 2 (Grün)
```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/
EXPOSE 80
```