Oto przykładowa procedura podmontowania zewnętrznego storage do deploy w Kubernetes za pomocą SMB w Azurze:

1. Utwórz nowy zasób magazynu w usłudze Azure Storage, wybierając opcję File Storage.

2. Przejdź do konfiguracji konta usługi Azure Storage i utwórz nową udział SMB.

3. Skonfiguruj sieć wirtualną Azure, aby umożliwić dostęp do udziału SMB z klastra Kubernetes.

4. Utwórz nowy plik konfiguracyjny YAML dla Kubernetes i dodaj w nim informacje o podłączeniu do udziału SMB. Przykładowy plik YAML może wyglądać tak:

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: smb-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany
  mountOptions:
    - vers=3.0
  smb:
    path: /mnt/share
    server: <adres_IP_sera_SMB>
    username: <nazwa_użytkownika>
    password: <hasło_użytkownika>
```

5. Utwórz PersistentVolumeClaim, który będzie używany do żądania zasobu woluminu SMB:

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: smb-pvc
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 1Gi
```

6. Dodaj volumen do manifestu aplikacji w Kubernetes:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: app
  template:
    metadata:
      labels:
        app: app
    spec:
      containers:
        - name: app
          image: <nazwa_obrazu>
          volumeMounts:
            - name: smb-vol
              mountPath: /mnt/share
      volumes:
        - name: smb-vol
          persistentVolumeClaim:
            claimName: smb-pvc
```

7. Uruchom nową aplikację w klastrze Kubernetes.

Dzięki powyższemu krokom zewnętrzny storage SMB zostanie podłączony do aplikacji w klastrze Kubernetes i będzie dostępny w podanym punkcie montowania.