# Einführung in Kubernetes

## Was ist Kubernetes?
Kubernetes (kurz: K8s) ist eine Open-Source-Plattform zur Automatisierung der Bereitstellung, Skalierung und Verwaltung von containerisierten Anwendungen. Es wurde von Google entwickelt und wird heute von der Cloud Native Computing Foundation (CNCF) betreut.

### Warum Kubernetes?
- Automatisierung von Deployment, Skalierung und Wartung
- Hohe Verfügbarkeit und Fehlertoleranz
- Plattformunabhängigkeit (On-Premise, Cloud, Hybrid)
- Verwaltung komplexer Microservice-Architekturen

---

## Grundlegende Kubernetes-Objekte

### Pods
Ein **Pod** ist die kleinste deploybare Einheit in Kubernetes. Er enthält einen oder mehrere Container, die sich Netzwerk und Speicherressourcen teilen.

**Beispiel:**
Ein Pod mit einem Nginx-Webserver:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webserver
spec:
  containers:
  - name: nginx
    image: nginx:latest
```

### ReplicaSet
Ein **ReplicaSet** sorgt dafür, dass eine bestimmte Anzahl identischer Pods im Cluster läuft.

**Beispiel:**
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: webserver-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: webserver
  template:
    metadata:
      labels:
        app: webserver
    spec:
      containers:
      - name: nginx
        image: nginx:latest
```

### Deployment
Ein **Deployment** verwaltet ReplicaSets und ermöglicht einfache Updates, Rollbacks und Versionierung.

**Beispiel:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webserver-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: webserver
  template:
    metadata:
      labels:
        app: webserver
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
```

---

## Netzwerk und Kommunikation

### Services
Ein **Service** ist ein permanenter Zugriffspunkt für einen oder mehrere Pods.

**Typen:**
- ClusterIP (Standard, nur intern)
- NodePort (Zugriff von außen über einen Port)
- LoadBalancer (externer Load Balancer, Cloud erforderlich)

**Beispiel:**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: webserver-service
spec:
  type: NodePort
  selector:
    app: webserver
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30007
```

---

## Skalierung und Anpassung

### Horizontal Pod Autoscaler (HPA)
Der HPA passt die Anzahl der Pods automatisch an, basierend auf der CPU- oder Speicherlast.

**Beispiel:**
```yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: webserver-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: webserver-deployment
  minReplicas: 2
  maxReplicas: 10
  targetCPUUtilizationPercentage: 50
```

---

## Weitere wichtige Objekte

### ConfigMap
Dient zur Speicherung von Konfigurationsdaten in Key-Value-Form.

**Beispiel:**
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_MODE: "production"
```

### Secret
Speichert sensible Daten (z. B. Passwörter, Tokens) verschlüsselt.

**Beispiel:**
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-credentials
stringData:
  username: admin
  password: geheim123
```

### Namespace
Ein Namespace teilt Ressourcen logisch, z. B. für Dev/Test/Prod-Umgebungen.

**Beispiel:**
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: development
```

---
