# k8s-varnish-test

Running Varnish and Nginx in Kubernetes.

1. Deploy manifests

```bash
kubectl apply -f nginx-deployment.yaml
kubectl apply -f nginx-service.yaml
kubectl apply -f varnish-configmap.yaml
kubectl apply -f varnish-deployment.yaml
kubectl apply -f varnish-service.yaml
```

2. Test Nginx
```bash
curl localhost:30080
```

3. Test Varnish
```bash
curl localhost:30081
```

See http://bit.ly/k8s-varnish-drupal for more info.
