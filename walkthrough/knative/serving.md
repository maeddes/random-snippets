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
      - image: docker.io/{username}/helloworld-java-spring
        env:
        - name: TARGET
          value: "Spring Boot Sample v1"
 ```
