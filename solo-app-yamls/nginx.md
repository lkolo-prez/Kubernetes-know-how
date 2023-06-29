Poniżej znajduje się zestaw plików YAML dla Deploymentu, Serwisu, PersistentVolumeClaim (PVC) i Ingress w Kubernetes, które razem tworzą prostą aplikację Nginx:

1. **Deployment**:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.19.3
        ports:
        - containerPort: 80
        volumeMounts:
        - name: nginx-storage
          mountPath: /usr/share/nginx/html
      volumes:
      - name: nginx-storage
        persistentVolumeClaim:
          claimName: nginx-pvc
```

2. **Service**:

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
  type: LoadBalancer
```

3. **PersistentVolumeClaim (PVC)**:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nginx-pvc
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
  name: nginx-ingress
spec:
  rules:
  - host: your-domain.com
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: nginx-service
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
