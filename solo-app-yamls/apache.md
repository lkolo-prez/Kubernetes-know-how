Poniżej znajduje się przykładowy zestaw plików YAML dla Deploymentu, Serwisu, PersistentVolumeClaim (PVC) i Ingress w Kubernetes, które razem tworzą prostą aplikację Apache:

1. **Deployment**:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: apache-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: apache
  template:
    metadata:
      labels:
        app: apache
    spec:
      containers:
      - name: apache
        image: httpd:2.4
        ports:
        - containerPort: 80
        volumeMounts:
        - name: apache-storage
          mountPath: /usr/local/apache2/htdocs/
      volumes:
      - name: apache-storage
        persistentVolumeClaim:
          claimName: apache-pvc
```

2. **Service**:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: apache-service
spec:
  selector:
    app: apache
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
  name: apache-pvc
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
  name: apache-ingress
spec:
  rules:
  - host: your-domain.com
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: apache-service
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

