# Azure-File-Share-In-AKS
Azure File Share in AKS
```
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=lxdevecommerce02 --from-literal=azurestorageaccountkey=bD32Z5Nn79v+Xk3L1Qi9bWGr1R1wR11WZ5ieA4QByIwVo7h4SMCSYChYVJTx/F7F59IDO5KSzNC5+AStprRT7w== -n dev02
```
```
kubectl apply -f pv-fileshare.yaml
```
```
kubectl apply -f pvc-fileshare.yaml
```
```
kubectl apply -f test-deployment.yaml
```

##### Reference 
- [Microsoft Offical Docs](https://docs.microsoft.com/en-us/azure/aks/azure-files-volume)
