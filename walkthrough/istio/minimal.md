Add Repo:

`helm repo add istio.io https://storage.googleapis.com/istio-release/releases/1.3.4/charts/`
`helm repo update`

Create namespace:

`kubectl create namespace istio-system`

`kubectl get crds | grep 'istio.io' | wc -l` -> 23

`helm template install/kubernetes/helm/istio --name istio --namespace istio-system \
    --values install/kubernetes/helm/istio/values-istio-minimal.yaml | kubectl apply -f -`


---

Download:

'curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.3.4 sh -\n'

'cd istio-1.3.4'

'export PATH=$PWD/bin:$PATH'

'for i in install/kubernetes/helm/istio-init/files/crd*yaml; do kubectl apply -f $i; done'





