
Poniżej znajduje się zestaw plików YAML dla Deploymentu, Serwisu, PersistentVolumeClaim (PVC) i Ingress w Kubernetes, które razem tworzą prostą aplikację Joomla:

1. **Deployment**:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: joomla-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: joomla
  template:
    metadata:
      labels:
        app: joomla
    spec:
      containers:
      - name: joomla
        image: joomla:3.9-php7.2-apache
        ports:
        - containerPort: 80
        volumeMounts:
        - name: joomla-storage
          mountPath: /var/www/html
      volumes:
      - name: joomla-storage
        persistentVolumeClaim:
          claimName: joomla-pvc
```

2. **Service**:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: joomla-service
spec:
  selector:
    app: joomla
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```

3. **PersistentVolumeClaim (PVC)**:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: joomla-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

4. **Ingress**:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: joomla-ingress
spec:
  rules:
  - host: your-domain.com
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: joomla-service
            port:
              number: 80
```

Zastąp `your-domain.com` w pliku Ingress swoją rzeczywistą domeną.

Aby utworzyć te zasoby w swoim klastrze Kubernetes, zapisz każdy z powyższych plików YAML na dysku (na przykład jako `deployment.yaml`, `service.yaml`, `pvc.yaml` i `ingress.yaml`), a następnie użyj polecenia `kubectl apply` dla każdego z nich:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f pvc.yaml
kubectl apply -f ingress.yaml
```

Pamiętaj, że musisz mieć zainstalowany i skonfigurowany `kubectl` oraz mieć dostęp do klastra Kubernetes, aby te polecenia zadziałały.

