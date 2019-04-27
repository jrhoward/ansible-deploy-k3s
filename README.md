## Install k3s

Installs k3s https://k3s.io/ with ansible in a multinode set up without docker. Uses containerd

## Usage

Review and update the inventory file `inventory-example.ini`

Then run:
```

ansible-playbook -i inventory-example.ini install.yaml

```

## Notes

- Tested on bare metal vanilla fedora 29 servers with one master and multiple nodes.

- Firewall is turned off

## Limitations/Issues

- Probably wont work on non redhat based distros

- k3s does not support multiple masters yet as of v0.4.0

- NFS client is not working

## Set up Persistent Storage

Use local storage provided by Rancher
```
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml

```

Deploy minio in distributed mode
```
KUBECONFIG=/etc/rancher/k3s/k3s.yaml helm install --set persistence.storageClass=local-path,mode=distributed,replicas=4 --name minio-distributed stable/minio
for i in {0..3} ; do kubectl patch pv $( kubectl get pvc export-minio-distributed-$i -o=jsonpath='{$.spec.volumeName}' ) -p '{"spec":{"persistentVolumeReclaimPolicy":"Retain"}}' ; done

```

Deploy dashboard
```

kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
kubectl -n kube-system get secret $(kubectl get sa admin-user -n kube-system -o=jsonpath='{$.secrets[0].name}' ) -o=jsonpath='{$.data.token}'

```
