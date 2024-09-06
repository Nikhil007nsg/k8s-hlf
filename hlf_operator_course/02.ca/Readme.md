# Certificate Authority (CA)

## Configuration

```bash
export SC=$(kubectl get sc -o=jsonpath='{.items[0].metadata.name}')
export SC=$(kubectl get sc -o=jsonpath='{.items[?(@.metadata.name=="local-storage")].metadata.name}')
export CA_IMAGE=hyperledger/fabric-ca
export CA_VERSION=1.5.7
export DOMAIN=blockchain-network.online #localho.st when running in kind cluster
export DOMAIN=localho.st
```

## CA Setup
# Create Storage-class
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: standard/local-storage
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
reclaimPolicy: Delete

### Org1 CA

# Create PersistentVolume 

apiVersion: v1
kind: PersistentVolume
metadata:
  name: org1-ca
  labels:
    type: nfs
spec:
  storageClassName: local-storage  # Use the appropriate storage class
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  nfs:
    path: /mnt/nfs_share/blockchain  # The path on the NFS server
    server: 172.27.22.181  # The NFS server's IP address


```bash
kubectl create ns org1

kubectl hlf ca create  --image=$CA_IMAGE --version=$CA_VERSION --storage-class=$SC --capacity=1Gi --name=org1-ca \
    --enroll-id=enroll --enroll-pw=enrollpw --hosts=org1-ca.$DOMAIN --istio-port=443 --namespace=org1
```

### Org2 CA

# Create PersistentVolume 

apiVersion: v1
kind: PersistentVolume
metadata:
  name: org2-ca
  labels:
    type: nfs
spec:
  storageClassName: local-storage  # Use the appropriate storage class
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  nfs:
    path: /mnt/nfs_share/blockchain  # The path on the NFS server
    server: 172.27.22.181  # The NFS server's IP address


```bash
kubectl create ns org2

kubectl hlf ca create  --image=$CA_IMAGE --version=$CA_VERSION --storage-class=$SC --capacity=1Gi --name=org2-ca \
    --enroll-id=enroll --enroll-pw=enrollpw --hosts=org2-ca.$DOMAIN --istio-port=443 --namespace=org2
```

### Orderer CA

```bash
kubectl create ns orderer

kubectl hlf ca create  --image=$CA_IMAGE --version=$CA_VERSION --storage-class=$SC --capacity=1Gi --name=orderer-ca \
    --enroll-id=enroll --enroll-pw=enrollpw --hosts=orderer-ca.$DOMAIN --istio-port=443 --namespace=orderer
```
