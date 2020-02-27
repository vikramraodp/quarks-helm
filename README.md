# quarks-helm repository

This repository provides the helm chart of the cf-operator.

The cf-operator manages the deployment of BOSH releases on Kubernetes.


## Usage

```
helm repo add quarks https://cloudfoundry-incubator.github.io/quarks-helm/
helm search repo quarks
helm install cf-operator quarks/cf-operator
```

## Example Using KinD

```
cat > kind-multi-node.yml <<EOF
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
EOF

kind create cluster --image kindest/node:v1.16.4 --config kind-multi-node.yml --name 116

helm repo add quarks https://cloudfoundry-incubator.github.io/quarks-helm/
helm install cf-operator quarks/cf-operator

kubectl config set-context --current --namespace staging
kubectl apply -f https://raw.githubusercontent.com/cloudfoundry-incubator/cf-operator/master/docs/examples/bosh-deployment/boshdeployment.yaml

helm delete cf-operator
kubectl delete customresourcedefinitions,validatingwebhookconfigurations,mutatingwebhookconfigurations --all

kind delete cluster --name 116
```
