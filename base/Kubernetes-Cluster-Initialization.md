
```markdown
# Kubernetes Cluster Initialization

To initialize your Kubernetes control-plane and join worker nodes to the cluster, follow the steps below:

1. Initialize the control-plane by running the following commands as a regular user:

   ```shell
   mkdir -p $HOME/.kube
   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   sudo chown $(id -u):$(id -g) $HOME/.kube/config
   ```

   These commands create the necessary directory and copy the admin configuration to your user's home directory.

2. Alternatively, if you are the root user, you can set the `KUBECONFIG` environment variable:

   ```shell
   export KUBECONFIG=/etc/kubernetes/admin.conf
   ```

   This command sets the `KUBECONFIG` environment variable to the location of the admin configuration file.

3. Deploy a pod network to enable communication between pods in the cluster. Choose one of the options listed at the [Kubernetes documentation](https://kubernetes.io/docs/concepts/cluster-administration/addons/) and run the following command:

   ```shell
   kubectl apply -f [podnetwork].yaml
   ```

   Replace `[podnetwork].yaml` with the chosen pod network configuration file.

   Note: This step is required to enable pod networking within the cluster.

4. To join worker nodes to the cluster, run the following command on each worker node as the root user:

   ```shell
   kubeadm join 10.10.10.10:6443 --token 58x1kr.7ww0xh17c7i6degs --discovery-token-ca-cert-hash sha256:aa16edb12c0a3541d8ca82ad1a4fa012d707e3bea695a54954a77cd2cb6a60f1
   ```

   Replace `10.10.10.10:6443` with the IP address and port of the control-plane node. Adjust the token and discovery token CA cert hash values accordingly.

   This command adds a worker node to the cluster.

By following these steps, you will initialize the Kubernetes control-plane and join worker nodes to the cluster.

For more information and detailed instructions, refer to the [Kubernetes documentation](https://kubernetes.io/docs/concepts/).
