Add Repo:

`helm repo add istio.io https://storage.googleapis.com/istio-release/releases/1.3.4/charts/`
`helm repo update`

Create namespace:

`kubectl create namespace istio-system`

Download (not sure if required):

`curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.3.4 sh -\n`

Install CRDs:

`helm template install/kubernetes/helm/istio-init --name istio-init --namespace istio-system | kubectl apply -f -`

Validate:

`kubectl get crds | grep 'istio.io' | wc -l` -> 23

Install minimal Istio setup:

`helm template install/kubernetes/helm/istio --name istio --namespace istio-system \
    --values install/kubernetes/helm/istio/values-istio-minimal.yaml | kubectl apply -f -`

Validate:

```
kubectl get svc -n istio-system

NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)                                                                                                                                      AGE
istio-citadel            ClusterIP      10.103.196.124   <none>        8060/TCP,15014/TCP                                                                                                                           5m2s
istio-galley             ClusterIP      10.98.222.183    <none>        443/TCP,15014/TCP,9901/TCP                                                                                                                   5m3s
istio-ingressgateway     LoadBalancer   10.109.44.207    localhost     15020:31439/TCP,80:31380/TCP,443:31390/TCP,31400:31400/TCP,15029:30040/TCP,15030:31193/TCP,15031:31194/TCP,15032:31458/TCP,15443:30839/TCP   5m3s
istio-pilot              ClusterIP      10.98.15.252     <none>        15010/TCP,15011/TCP,8080/TCP,15014/TCP                                                                                                       5m2s
istio-policy             ClusterIP      10.96.195.190    <none>        9091/TCP,15004/TCP,15014/TCP                                                                                                                 5m2s
istio-sidecar-injector   ClusterIP      10.99.10.38      <none>        443/TCP,15014/TCP                                                                                                                            5m2s
istio-telemetry          ClusterIP      10.111.42.165    <none>        9091/TCP,15004/TCP,15014/TCP,42422/TCP                                                                                                       5m2s
prometheus               ClusterIP      10.108.79.243    <none>        9090/TCP                                                                                                                                     5m2s
```


---

Download:

'curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.3.4 sh -\n'

'cd istio-1.3.4'

'export PATH=$PWD/bin:$PATH'

'for i in install/kubernetes/helm/istio-init/files/crd*yaml; do kubectl apply -f $i; done'





