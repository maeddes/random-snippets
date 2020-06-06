curl -L https://istio.io/downloadIstio | sh -\n
cd istio-1.6.1
export PATH=$PWD/bin:$PATH
istioctl install --set profile=default

follow rest here:

https://knative.dev/docs/install/any-kubernetes-cluster/
