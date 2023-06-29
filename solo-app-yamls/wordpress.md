Poniżej znajduje się zestaw plików YAML dla Deploymentu, Serwisu i Ingress w Kubernetes, które razem tworzą prostą aplikację WordPress. Zauważ, że WordPress wymaga bazy danych MySQL do działania. W tym przykładzie zakładam, że masz już działającą zdalną bazę danych MySQL, do której WordPress może się połączyć.

1. **Deployment**:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: wordpress
  template:
    metadata:
      labels:
        app: wordpress
    spec:
      containers:
      - name: wordpress
        image: wordpress:5.7.2-php7.4-apache
        ports:
        - containerPort: 80
        env:
        - name: WORDPRESS_DB_HOST
          value: your-db-host
        - name: WORDPRESS_DB_USER
          value: your-db-user
        - name: WORDPRESS_DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: your-db-secret
              key: password
        - name: WORDPRESS_DB_NAME
          value: your-db-name
```

Zastąp `your-db-host`, `your-db-user` i `your-db-name` odpowiednimi wartościami dla twojej bazy danych. `your-db-secret` to nazwa Secretu w Kubernetes, który zawiera hasło do bazy danych. Możesz utworzyć taki Secret za pomocą polecenia `kubectl create secret`.

2. **Service**:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: wordpress-service
spec:
  selector:
    app: wordpress
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```

3. **Ingress**:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: wordpress-ingress
spec:
  rules:
  - host: your-domain.com
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: wordpress-service
            port:
              number: 80
```

Zastąp `your-domain.com` w pliku Ingress swoją rzeczywistą domeną.

Aby utworzyć te zasoby w swoim klastrze Kubernetes, zapisz każdy z powyższych plików YAML na dysku (na przykład jako `deployment.yaml`, `service.yaml` i `ingress.yaml`), a następnie użyj polecenia `kubectl apply` dla każdego z nich:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml
```

