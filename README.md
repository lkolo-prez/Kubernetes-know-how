# Kubernetes-know-how

This repository serves as my knowledge base for Kubernetes technology. It contains various YAML files based on my experiences primarily with the Azure Kubernetes Service (AKS) distribution. Here, you will find helpful resources, configurations, and examples to aid in understanding and working with Kubernetes.

## What is Kubernetes?

Kubernetes, often referred to as K8s, is an open-source container orchestration platform. It simplifies the management and deployment of containerized applications at scale. With Kubernetes, you can efficiently manage clusters of containers, automate application scaling, and ensure high availability.

Kubernetes was initially developed by Google engineers and was open-sourced in 2014. It has gained significant popularity and has become the de facto standard for container orchestration. It provides a robust and flexible platform for deploying and managing containerized applications across various environments, including on-premises, cloud providers, or hybrid setups.

## Examples

Here are a few simple examples of YAML code commonly used in Kubernetes:

1. **Deployment**:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-app-container
          image: my-app-image:latest
          resources:
            limits:
              cpu: "1"
              memory: "1Gi"
            requests:
              cpu: "500m"
              memory: "400Mi"
          ports:
            - containerPort: 8080
```

2. **Service**:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```

3. **Ingress**:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-app-ingress
spec:
  rules:
    - host: myapp.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: my-app-service
                port:
                  number: 80
```

Feel free to explore more YAML examples within this repository to learn and experiment with different Kubernetes concepts and configurations.

## Contributing

If you have any suggestions, improvements, or additional examples that you would like to contribute to this knowledge base, please feel free to submit a pull request. Your contributions are greatly appreciated!

## License

This repository is licensed under the [MIT License](LICENSE).
