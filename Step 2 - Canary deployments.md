## Canary deploy canary-v2

```bash
Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ ./canary.sh
configmap/canary-config-v2 created
deployment.apps/canary-v2 created
service/canary-svc created
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
canary-svc   ClusterIP   172.20.231.215   <none>        80/TCP    3s
deployment.apps/canary-v2 scaled
deployment.apps/canary-v1 scaled
Waiting for deployment "canary-v2" rollout to finish: 1 of 2 updated replicas are available...
deployment "canary-v2" successfully rolled out
V1 PODS: 2
V2 PODS: 2
Canary deployment of v2 successful
```

## Curl service 10 times

```bash
Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl run -i --tty --rm debug --image=curlimages/curl --restart=Never -- sh
If you don't see a command prompt, try pressing enter.
/ $ curl 172.20.231.215
<html>
<h1>This is version 2</h1>
</html>
/ $ curl 172.20.231.215
<html>
<h1>This is version 2</h1>
</html>
/ $ curl 172.20.231.215
<html>
<h1>This is version 2</h1>
</html>
/ $ curl 172.20.231.215
<html>
<h1>This is version 1</h1>
</html>
/ $ curl 172.20.231.215
<html>
<h1>This is version 1</h1>
</html>
/ $ curl 172.20.231.215
<html>
<h1>This is version 2</h1>
</html>
/ $ curl 172.20.231.215
<html>
<h1>This is version 1</h1>
</html>
/ $ curl 172.20.231.215
<html>
<h1>This is version 1</h1>
</html>
/ $ curl 172.20.231.215
<html>
<h1>This is version 2</h1>
</html>
/ $ curl 172.20.231.215
<html>
<h1>This is version 1</h1>
</html>
```

## Output of kubectl get pods

```bash
Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl get pods --all-namespaces
NAMESPACE     NAME                           READY   STATUS    RESTARTS   AGE
kube-system   aws-node-p2w69                 1/1     Running   0          80m
kube-system   aws-node-sxkck                 1/1     Running   0          80m
kube-system   coredns-f47955f89-22sfw        1/1     Running   0          85m
kube-system   coredns-f47955f89-kvlzj        1/1     Running   0          85m
kube-system   kube-proxy-mdc7k               1/1     Running   0          80m
kube-system   kube-proxy-spp7s               1/1     Running   0          80m
udacity       blue-8475cbdf46-ftf8c          1/1     Running   0          75m
udacity       blue-8475cbdf46-m544d          1/1     Running   0          75m
udacity       blue-8475cbdf46-sz8ql          1/1     Running   0          75m
udacity       canary-v1-64598c676f-fm5hb     1/1     Running   0          75m
udacity       canary-v1-64598c676f-jcvfs     1/1     Running   0          2m35s
udacity       canary-v2-5dc9c56687-2v96m     1/1     Running   0          93s
udacity       canary-v2-5dc9c56687-d5dgh     1/1     Running   0          78s
udacity       hello-world-794458d64d-n5gzj   1/1     Running   0          34m
```