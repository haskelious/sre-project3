## Troubelshooting log

```bash
Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl get pods
NAME                          READY   STATUS             RESTARTS   AGE
blue-8475cbdf46-ftf8c         1/1     Running            0          39m
blue-8475cbdf46-m544d         1/1     Running            0          39m
blue-8475cbdf46-sz8ql         1/1     Running            0          39m
canary-v1-64598c676f-22lc9    1/1     Running            0          39m
canary-v1-64598c676f-fm5hb    1/1     Running            0          39m
canary-v1-64598c676f-sj4lh    1/1     Running            0          39m
hello-world-d696c5567-5bcq9   0/1     CrashLoopBackOff   4          3m9s

Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl get pods
NAME                          READY   STATUS             RESTARTS   AGE
blue-8475cbdf46-ftf8c         1/1     Running            0          39m
blue-8475cbdf46-m544d         1/1     Running            0          39m
blue-8475cbdf46-sz8ql         1/1     Running            0          39m
canary-v1-64598c676f-22lc9    1/1     Running            0          39m
canary-v1-64598c676f-fm5hb    1/1     Running            0          39m
canary-v1-64598c676f-sj4lh    1/1     Running            0          39m
hello-world-d696c5567-5bcq9   0/1     CrashLoopBackOff   4          3m18s

Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl logs hello-world-d696c5567-5bcq9
Ready to receive requests on 9000
 * Serving Flask app 'main' (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on all addresses.
   WARNING: This is a development server. Do not use it in a production deployment.
 * Running on http://10.100.2.84:9000/ (Press CTRL+C to quit)
Failed health check you want to ping /healthz
10.100.2.211 - - [07/Feb/2023 20:54:36] "GET /nginx_status HTTP/1.1" 500 -
Failed health check you want to ping /healthz
10.100.2.211 - - [07/Feb/2023 20:54:38] "GET /nginx_status HTTP/1.1" 500 -
Failed health check you want to ping /healthz
10.100.2.211 - - [07/Feb/2023 20:54:40] "GET /nginx_status HTTP/1.1" 500 -

Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl apply -f starter/apps/hello-world/hello.yml
deployment.apps/hello-world configured
service/hello-world unchanged

Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl get pods
NAME                           READY   STATUS        RESTARTS   AGE
blue-8475cbdf46-ftf8c          1/1     Running       0          40m
blue-8475cbdf46-m544d          1/1     Running       0          40m
blue-8475cbdf46-sz8ql          1/1     Running       0          40m
canary-v1-64598c676f-22lc9     1/1     Running       0          40m
canary-v1-64598c676f-fm5hb     1/1     Running       0          40m
canary-v1-64598c676f-sj4lh     1/1     Running       0          40m
hello-world-794458d64d-n5gzj   1/1     Running       0          11s
hello-world-d696c5567-5bcq9    1/1     Terminating   5          4m23s

Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl get pods
NAME                           READY   STATUS        RESTARTS   AGE
blue-8475cbdf46-ftf8c          1/1     Running       0          41m
blue-8475cbdf46-m544d          1/1     Running       0          41m
blue-8475cbdf46-sz8ql          1/1     Running       0          41m
canary-v1-64598c676f-22lc9     1/1     Running       0          41m
canary-v1-64598c676f-fm5hb     1/1     Running       0          41m
canary-v1-64598c676f-sj4lh     1/1     Running       0          41m
hello-world-794458d64d-n5gzj   1/1     Running       0          26s
hello-world-d696c5567-5bcq9    1/1     Terminating   6          4m38s

Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl logs hello-world-794458d64d-n5gzj
Ready to receive requests on 9000
 * Serving Flask app 'main' (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on all addresses.
   WARNING: This is a development server. Do not use it in a production deployment.
 * Running on http://10.100.2.72:9000/ (Press CTRL+C to quit)
Healthy!
10.100.2.211 - - [07/Feb/2023 20:56:23] "GET /healthz HTTP/1.1" 200 -
Healthy!
10.100.2.211 - - [07/Feb/2023 20:56:25] "GET /healthz HTTP/1.1" 200 -
Healthy!
10.100.2.211 - - [07/Feb/2023 20:56:27] "GET /healthz HTTP/1.1" 200 -
Healthy!
10.100.2.211 - - [07/Feb/2023 20:56:29] "GET /healthz HTTP/1.1" 200 -
Healthy!
10.100.2.211 - - [07/Feb/2023 20:56:31] "GET /healthz HTTP/1.1" 200 -
Healthy!
10.100.2.211 - - [07/Feb/2023 20:56:33] "GET /healthz HTTP/1.1" 200 -
Healthy!
10.100.2.211 - - [07/Feb/2023 20:56:35] "GET /healthz HTTP/1.1" 200 -
Healthy!
10.100.2.211 - - [07/Feb/2023 20:56:37] "GET /healthz HTTP/1.1" 200 -
Healthy!
10.100.2.211 - - [07/Feb/2023 20:56:39] "GET /healthz HTTP/1.1" 200 -
10.100.2.211 - - [07/Feb/2023 20:56:41] "GET /healthz HTTP/1.1" 200 -
Healthy!
Healthy!
10.100.2.211 - - [07/Feb/2023 20:56:43] "GET /healthz HTTP/1.1" 200 -
Healthy!
10.100.2.211 - - [07/Feb/2023 20:56:45] "GET /healthz HTTP/1.1" 200 -
10.100.2.211 - - [07/Feb/2023 20:56:47] "GET /healthz HTTP/1.1" 200 -
Healthy!
Healthy!
10.100.2.211 - - [07/Feb/2023 20:56:49] "GET /healthz HTTP/1.1" 200 -
10.100.2.211 - - [07/Feb/2023 20:56:51] "GET /healthz HTTP/1.1" 200 -
Healthy!
Healthy!
10.100.2.211 - - [07/Feb/2023 20:56:53] "GET /healthz HTTP/1.1" 200 -
10.100.2.211 - - [07/Feb/2023 20:56:55] "GET /healthz HTTP/1.1" 200 -
Healthy!

Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$
```

## Modified file `hello.yml`:

```diff
$ git diff starter/apps/hello-world/
diff --git a/starter/apps/hello-world/hello.yml b/starter/apps/hello-world/hello.yml
index 8b6bed1..3c5a82c 100755
--- a/starter/apps/hello-world/hello.yml
+++ b/starter/apps/hello-world/hello.yml
@@ -25,7 +25,7 @@ spec:
           livenessProbe:
             httpGet:
               port: 9000
-              path: /nginx_status
+              path: /healthz
             initialDelaySeconds: 2
             periodSeconds: 2
       nodeSelector:
```
