## tutorial

https://starkandwayne.com/blog/running-cloud-foundry-on-kubernetes-using-kubecf/

## kind

https://kind.sigs.k8s.io/
https://medium.com/@jmpinto/deploying-cloudfoundry-on-a-local-kubernetes-9103a57bf713
https://hub.docker.com/r/kindest/node/tags
https://github.com/kubernetes-sigs/kind/releases

kind create cluster --name kubecf --image kindest/node:latest
kind create cluster --name kubecf --image kindest/node:v1.15.11

node_ip=$(kubectl get node kind-control-plane --output jsonpath='{ .status.addresses[?(@.type == "InternalIP")].address }')


## kubecf

### cf operator

https://github.com/cloudfoundry-incubator/kubecf/releases
https://kubecf.s3.amazonaws.com/index.html
https://cf-operators.s3.amazonaws.com/helm-charts/index.html

kubectl create ns cf-operator

helm install cf-operator --namespace cf-operator --set "global.operator.watchNamespace=kubecf"     https://s3.amazonaws.com/cf-operators/release/helm-charts/cf-operator-4.5.6%2B0.gffc6f942.tgz

### kubecf

https://github.com/cloudfoundry-incubator/kubecf

cat << _EOF_  > values.yaml
system_domain: ${node_ip}.nip.io
services:
  router:
    externalIPs:
    - ${node_ip}
kube:
  service_cluster_ip_range: 10.100.0.0/16
  pod_cluster_ip_range: 192.16.0.0/16
features:
  eirini:
    enabled: true
_EOF_

helm install kubecf --namespace kubecf --values values.yaml https://github.com/cloudfoundry-incubator/kubecf/releases/download/v2.2.2/kubecf-v2.2.2.tgz

### cf-for-k8s

https://starkandwayne.com/blog/deploy-cf-for-k8s-to-google-in-10-minutes/
https://tanzu.vmware.com/developer/guides/kubernetes/cf4k8s-gs/
https://github.com/cloudfoundry/cf-for-k8s/blob/master/docs/deploy.md
https://github.com/dbbaskette/tas-on-kind
https://github.com/nrekretep/k8s-dev-cluster/blob/master/cf-for-k8s.md
