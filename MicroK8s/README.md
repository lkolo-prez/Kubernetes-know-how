
# MicroK8s - Podstawowe Instrukcje

## Instalacja MicroK8s

### Ubuntu

```bash
sudo snap install microk8s --classic
```

### Windows

1. Zainstaluj [Multipass](https://multipass.run/)
2. Uruchom Multipass i stwórz nową instancję Ubuntu:

```bash
multipass launch --name ubuntu-lts
```

3. Zainstaluj MicroK8s na instancji Ubuntu:

```bash
multipass exec ubuntu-lts -- sudo snap install microk8s --classic
```

### MacOS

1. Zainstaluj [Multipass](https://multipass.run/)
2. Uruchom Multipass i stwórz nową instancję Ubuntu:

```bash
multipass launch --name ubuntu-lts
```

3. Zainstaluj MicroK8s na instancji Ubuntu:

```bash
multipass exec ubuntu-lts -- sudo snap install microk8s --classic
```

## Podstawy MicroK8s

Uruchomienie klastra:

```bash
microk8s start
```

Zatrzymanie klastra:

```bash
microk8s stop
```

Sprawdzenie statusu klastra:

```bash
microk8s status
```

## Wdrażanie aplikacji na MicroK8s

Stwórz plik `nginx-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

Wdrożenie aplikacji:

```bash
microk8s kubectl apply -f nginx-deployment.yaml
```

Sprawdzenie statusu wdrożenia:

```bash
microk8s kubectl get deployments
```

## Konfiguracja sieci w MicroK8s

Stwórz plik `nginx-service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

Utworzenie usługi:

```bash
microk8s kubectl apply -f nginx-service.yaml
```

Sprawdzenie statusu usługi:

```bash
microk8s kubectl get services
```

## Praca z woluminami danych w MicroK8s

Stwórz plik `nginx-pv-pvc.yaml`:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: nginx-pv
spec:
  capacity:
    storage: 1Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName:

Przykro mi, ale moja odpowiedź przekroczyła limit znaków. Kontynuuję poniżej:

```yaml
  local-storage
  hostPath:
    path: "/mnt/data"

---

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nginx-pvc
spec:
  storageClassName: local-storage
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

Utworzenie woluminu trwałego i żądania woluminu trwałego:

```bash
microk8s kubectl apply -f nginx-pv-pvc.yaml
```

Sprawdzenie statusu PV i PVC:

```bash
microk8s kubectl get pv
microk8s kubectl get pvc
```

## Zabezpieczanie klastra MicroK8s

Utwórz plik `rbac.yaml`:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: user
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: user-role
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: user-rolebinding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: user-role
subjects:
- kind: ServiceAccount
  name: user
```

Zastosuj RBAC:

```bash
microk8s kubectl apply -f rbac.yaml
```

## Używanie dodatków MicroK8s

Włącz dodatek Istio:

```bash
microk8s enable istio
```

Sprawdź status dodatku Istio:

```bash
microk8s istioctl version
```

## Najlepsze praktyki i porady dotyczące MicroK8s

1. Regularnie aktualizuj MicroK8s do najnowszej wersji.
2. Regularnie sprawdzaj status klastra i monitoruj zużycie zasobów.
3. Regularnie twórz kopie zapasowe swoich danych.
4. Używaj narzędzi do zarządzania konfiguracją, takich jak Helm, do zarządzania złożonymi wdrożeniami.
5. Używaj dodatków MicroK8s do łatwego włączania zaawansowanych funkcji Kubernetes.