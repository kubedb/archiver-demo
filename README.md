These 4 componenets need to be installed in your cluster : 

# Sidekick

helm pull oci://ghcr.io/appscode-charts/sidekick --version v2023.12.11

helm upgrade -i sidekick sidekick-v2023.12.11.tgz \
  -n kubedb --create-namespace \
  --wait --burst-limit=10000 --debug


# KubeStash

helm pull oci://ghcr.io/appscode-charts/kubestash --version v2023.12.1

helm upgrade -i kubestash kubestash-v2023.12.1.tgz \
  --namespace kubedb \
  --set-file global.license=/path/to/the/license.txt \
  --wait --burst-limit=10000 --debug

# KubeDB

helm uninstall kubedb -n kubedb
helm pull oci://ghcr.io/appscode-charts/kubedb --version v2023.12.11

kubectl apply -f https://github.com/kubedb/installer/raw/v2023.12.11/crds/kubedb-catalog-crds.yaml

helm upgrade -i kubedb kubedb-v2023.12.11.tgz \
  --namespace kubedb \
  --set kubedb-kubestash-catalog.enabled=true \
  --set-file global.license=/path/to/the/license.txt \
  --wait --burst-limit=10000 --debug


# VolumeSnapshotter

If You are using cloud providers like AWS, Google cloud etc who already make volume-snapshotter available in youe cluster, You don't need to follow this step.
Otherwise, You need to install an external-snapshotter. 


https://github.com/kubernetes-csi/external-snapshotter/tree/release-5.0

wget https://github.com/kubernetes-csi/external-snapshotter/archive/refs/tags/v5.0.1.zip

```
unzip v5.0.1.zip
cd 

kubectl kustomize client/config/crd | kubectl create -f -
kubectl -n kube-system kustomize deploy/kubernetes/snapshot-controller | kubectl create -f -
kubectl kustomize deploy/kubernetes/csi-snapshotter | kubectl create -f -
```
