Watch both knative and kubernetes relevant artifacts:

`watch -n0.1 kubectl get configuration,revision,deployment,replicaset,route,pod`

Use kn cli to deploy first application:

```
kn service create quarkus-knative --image=maeddes/quarkus-hello
```
```
Service 'quarkus-knative' successfully created in namespace 'default'.
Waiting for service 'quarkus-knative' to become ready ...OK

Service URL:
http://quarkus-knative.default.example.com
```

the watch command should look like this:

```
Every 0.1s: kubectl get configuration,revision,deployment,replicaset,route,pod    macbookmhs: Wed Oct 30 12:51:13 2019

NAME                                                LATESTCREATED           LATESTREADY             READY   REASON
configuration.serving.knative.dev/quarkus-knative   quarkus-knative-6bwrv   quarkus-knative-6bwrv   True

NAME                                                 CONFIG NAME       K8S SERVICE NAME        GENERATION   READY   RE
ASON
revision.serving.knative.dev/quarkus-knative-6bwrv   quarkus-knative   quarkus-knative-6bwrv   1            True

NAME                                                     READY   UP-TO-DATE   AVAILABLE   AGE
deployment.extensions/quarkus-knative-6bwrv-deployment   1/1     1            1           54s

NAME                                                                DESIRED   CURRENT   READY   AGE
replicaset.extensions/quarkus-knative-6bwrv-deployment-6c59599f5b   1         1         1       55s

NAME                                        URL                                          READY   REASON
route.serving.knative.dev/quarkus-knative   http://quarkus-knative.default.example.com   True

NAME                                                    READY   STATUS    RESTARTS   AGE
pod/quarkus-knative-6bwrv-deployment-6c59599f5b-4n98v   2/2     Running   0          55s
```

After a while watch it going back to zero instances

```
NAME                                                     READY   UP-TO-DATE   AVAILABLE   AGE
deployment.extensions/quarkus-knative-6bwrv-deployment   0/0     0            0           9m29s

NAME                                                                DESIRED   CURRENT   READY   AGE
replicaset.extensions/quarkus-knative-6bwrv-deployment-6c59599f5b   0         0         0       9m29s

NAME                                        URL                                          READY   REASON
route.serving.knative.dev/quarkus-knative   http://quarkus-knative.default.example.com   True
```

(New window) Identify route

```
kubectl get route quarkus-knative -o jsonpath="{.status.url}"
http://quarkus-knative.default.example.com
```

Identify IP/host if Istio Ingress gateway

```
kubectl get svc istio-ingressgateway -n istio-system -o jsonpath="{.status.loadBalancer.ingress[0].ip}"
51.145.133.17
```

Compose it to cURL call

```
curl -H "Host: quarkus-knative.default.example.com" 51.145.133.17/hello
Hello World!
```

Wrap into while loop

```
while true; 
  do 
    curl -H "Host: quarkus-knative.default.example.com" 51.145.133.17/hello;
    sleep 1;
    echo;
  done

Hello World!
Hello World!
Hello World!
Hello World!
```

Kill instances

```
curl -H "Host: quarkus-knative.default.example.com" 51.145.133.17/fail
dial tcp 127.0.0.1:8080: connect: connection refused
```

Log output

`stern "quarkus-knative*"`






Alternative use yaml file:

```yaml
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: helloworld-java-spring
  namespace: default
spec:
  template:
    spec:
      containers:
      - image: docker.io/maeddes/helloworld-java-spring
        env:
        - name: property
          value: "Spring Boot Sample v1"
 ```
