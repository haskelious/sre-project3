## Deployment of bloatware microservice

```bash
Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl apply -f starter/apps/bloatware/bloatware.yml
deployment.apps/bloaty-mcbloatface created
```

## Reason why the deployment is not successful

```bash
Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl get pods
NAME                                  READY   STATUS        RESTARTS   AGE
bloaty-mcbloatface-58c98b98d8-49gzq   1/1     Running       0          32s
bloaty-mcbloatface-58c98b98d8-4j9d9   1/1     Running       0          32s
bloaty-mcbloatface-58c98b98d8-ct8zz   1/1     Running       0          32s
bloaty-mcbloatface-58c98b98d8-dd42q   1/1     Running       0          32s
bloaty-mcbloatface-58c98b98d8-hwc2k   1/1     Running       0          32s
bloaty-mcbloatface-58c98b98d8-hztn4   0/1     Pending       0          32s
bloaty-mcbloatface-58c98b98d8-j5vgc   0/1     Pending       0          32s
bloaty-mcbloatface-58c98b98d8-lm49c   1/1     Running       0          32s
bloaty-mcbloatface-58c98b98d8-mbbgd   1/1     Running       0          32s
bloaty-mcbloatface-58c98b98d8-mj6r2   1/1     Running       0          32s
bloaty-mcbloatface-58c98b98d8-q4r57   0/1     Pending       0          32s
bloaty-mcbloatface-58c98b98d8-r8hh5   1/1     Running       0          32s
bloaty-mcbloatface-58c98b98d8-rtmbr   0/1     Pending       0          32s
bloaty-mcbloatface-58c98b98d8-sgrq2   1/1     Running       0          32s
bloaty-mcbloatface-58c98b98d8-vtm6g   1/1     Running       0          32s
bloaty-mcbloatface-58c98b98d8-x6mrc   1/1     Running       0          32s
bloaty-mcbloatface-58c98b98d8-xwvnq   0/1     Pending       0          32s
canary-v1-64598c676f-6gcm8            0/1     Terminating   0          14m
canary-v1-64598c676f-fm5hb            1/1     Terminating   0          144m
canary-v1-64598c676f-jcvfs            1/1     Terminating   0          72m
canary-v1-64598c676f-mnln5            1/1     Running       0          12m
canary-v1-64598c676f-rhrcj            1/1     Running       0          12m
canary-v2-5dc9c56687-4lvhg            1/1     Running       0          12m
canary-v2-5dc9c56687-bm7xk            0/1     Terminating   0          14m
canary-v2-5dc9c56687-h4lql            1/1     Terminating   0          28m
canary-v2-5dc9c56687-mvk8z            1/1     Terminating   0          63m
canary-v2-5dc9c56687-zxcs8            1/1     Running       0          12m
green-5cdd96c9b4-28xgt                1/1     Terminating   0          28m
green-5cdd96c9b4-2rm24                1/1     Running       0          12m
green-5cdd96c9b4-59wq6                1/1     Terminating   0          28m
green-5cdd96c9b4-b2pg7                1/1     Terminating   0          28m
green-5cdd96c9b4-fjh49                1/1     Running       0          12m
green-5cdd96c9b4-kzf98                0/1     Terminating   0          14m
green-5cdd96c9b4-pxdrh                0/1     Terminating   0          14m
green-5cdd96c9b4-qb7vs                1/1     Running       0          12m
hello-world-794458d64d-4gkrj          1/1     Running       0          12m
hello-world-794458d64d-n5gzj          1/1     Terminating   0          104m
hello-world-794458d64d-tsxmw          0/1     Terminating   0          14m

Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl describe pod bloaty-mcbloatface-58c98b98d8-hztn4
Name:             bloaty-mcbloatface-58c98b98d8-hztn4
Namespace:        udacity
Priority:         0
Service Account:  default
Node:             <none>
Labels:           app=bloaty-mcbloatface
                  pod-template-hash=58c98b98d8
Annotations:      kubernetes.io/psp: eks.privileged
Status:           Pending
IP:
IPs:              <none>
Controlled By:    ReplicaSet/bloaty-mcbloatface-58c98b98d8
Containers:
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-hlfjr (ro)
Conditions:
  Type           Status
  PodScheduled   False
Volumes:
  kube-api-access-hlfjr:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   Burstable
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason            Age                From               Message
  ----     ------            ----               ----               -------
  Warning  FailedScheduling  75s (x2 over 77s)  default-scheduler  0/4 nodes are available: 1 Insufficient cpu, 1 Too many pods, 2 node(s) were unschedulable.
```

## Resolution steps

```bash
Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ aws iam list-open-id-connect-providers
{
    "OpenIDConnectProviderList": []
}

Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ eksctl utils associate-iam-oidc-provider --cluster udacity-cluster --approve --region=us-east-2
2023-02-08 12:06:10 [ℹ]  will create IAM Open ID Connect provider for cluster "udacity-cluster" in "us-east-2"
2023-02-08 12:06:11 [✔]  created IAM Open ID Connect provider for cluster "udacity-cluster" in "us-east-2"

Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ eksctl create iamserviceaccount --name cluster-autoscaler --namespace kube-system --cluster udacity-cluster --attach-policy-arn "arn:aws:iam::043454495621:policy/udacity-k8s-autoscale" --approve --override-existing-serviceaccounts --region=us-east-2
2023-02-08 13:20:48 [ℹ]  1 iamserviceaccount (kube-system/cluster-autoscaler) was included (based on the include/exclude rules)
2023-02-08 13:20:48 [!]  metadata of serviceaccounts that exist in Kubernetes will be updated, as --override-existing-serviceaccounts was set
2023-02-08 13:20:48 [ℹ]  1 task: {
    2 sequential sub-tasks: {
        create IAM role for serviceaccount "kube-system/cluster-autoscaler",
        create serviceaccount "kube-system/cluster-autoscaler",
    } }2023-02-08 13:20:48 [ℹ]  building iamserviceaccount stack "eksctl-udacity-cluster-addon-iamserviceaccount-kube-system-cluster-autoscaler"
2023-02-08 13:20:48 [ℹ]  deploying stack "eksctl-udacity-cluster-addon-iamserviceaccount-kube-system-cluster-autoscaler"
2023-02-08 13:20:48 [ℹ]  waiting for CloudFormation stack "eksctl-udacity-cluster-addon-iamserviceaccount-kube-system-cluster-autoscaler"
2023-02-08 13:21:19 [ℹ]  waiting for CloudFormation stack "eksctl-udacity-cluster-addon-iamserviceaccount-kube-system-cluster-autoscaler"
2023-02-08 13:21:19 [ℹ]  created serviceaccount "kube-system/cluster-autoscaler"

Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl get sa cluster-autoscaler --namespace kube-system
NAME                 SECRETS   AGE
cluster-autoscaler   1         45s

Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl apply -f starter/infra/cluster_autoscale.yml
clusterrole.rbac.authorization.k8s.io/cluster-autoscaler created
role.rbac.authorization.k8s.io/cluster-autoscaler created
clusterrolebinding.rbac.authorization.k8s.io/cluster-autoscaler created
rolebinding.rbac.authorization.k8s.io/cluster-autoscaler created
deployment.apps/cluster-autoscaler created

Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl -n kube-system logs -f deployment/cluster-autoscaler
I0208 12:29:27.149496       1 flags.go:52] FLAG: --add-dir-header="false"
I0208 12:29:27.149552       1 flags.go:52] FLAG: --address=":8085"
I0208 12:29:27.149557       1 flags.go:52] FLAG: --alsologtostderr="false"
I0208 12:29:27.149561       1 flags.go:52] FLAG: --aws-use-static-instance-list="false"
I0208 12:29:27.149565       1 flags.go:52] FLAG: --balance-similar-node-groups="true"
I0208 12:29:27.149569       1 flags.go:52] FLAG: --balancing-ignore-label="[]"
I0208 12:29:27.149574       1 flags.go:52] FLAG: --cloud-config=""
I0208 12:29:27.149578       1 flags.go:52] FLAG: --cloud-provider="aws"
I0208 12:29:27.149587       1 flags.go:52] FLAG: --cloud-provider-gce-l7lb-src-cidrs="130.211.0.0/22,35.191.0.0/16"
I0208 12:29:27.149593       1 flags.go:52] FLAG: --cloud-provider-gce-lb-src-cidrs="130.211.0.0/22,209.85.152.0/22,209.85.204.0/22,35.191.0.0/16"
I0208 12:29:27.149600       1 flags.go:52] FLAG: --cluster-name=""
I0208 12:29:27.149604       1 flags.go:52] FLAG: --clusterapi-cloud-config-authoritative="false"
I0208 12:29:27.149608       1 flags.go:52] FLAG: --cordon-node-before-terminating="false"
I0208 12:29:27.149611       1 flags.go:52] FLAG: --cores-total="0:320000"
I0208 12:29:27.149615       1 flags.go:52] FLAG: --daemonset-eviction-for-empty-nodes="false"
I0208 12:29:27.149619       1 flags.go:52] FLAG: --estimator="binpacking"
I0208 12:29:27.149647       1 flags.go:52] FLAG: --expander="least-waste"
I0208 12:29:27.149652       1 flags.go:52] FLAG: --expendable-pods-priority-cutoff="-10"
I0208 12:29:27.149656       1 flags.go:52] FLAG: --gce-concurrent-refreshes="1"
I0208 12:29:27.149661       1 flags.go:52] FLAG: --gpu-total="[]"
I0208 12:29:27.149665       1 flags.go:52] FLAG: --ignore-daemonsets-utilization="false"
I0208 12:29:27.149669       1 flags.go:52] FLAG: --ignore-mirror-pods-utilization="false"
I0208 12:29:27.149673       1 flags.go:52] FLAG: --ignore-taint="[]"
I0208 12:29:27.149678       1 flags.go:52] FLAG: --kubeconfig=""
I0208 12:29:27.149681       1 flags.go:52] FLAG: --kubernetes=""
I0208 12:29:27.149686       1 flags.go:52] FLAG: --leader-elect="true"
I0208 12:29:27.149692       1 flags.go:52] FLAG: --leader-elect-lease-duration="15s"
I0208 12:29:27.149699       1 flags.go:52] FLAG: --leader-elect-renew-deadline="10s"
I0208 12:29:27.149721       1 flags.go:52] FLAG: --leader-elect-resource-lock="leases"
I0208 12:29:27.149728       1 flags.go:52] FLAG: --leader-elect-resource-name="cluster-autoscaler"
I0208 12:29:27.149732       1 flags.go:52] FLAG: --leader-elect-resource-namespace=""
I0208 12:29:27.149736       1 flags.go:52] FLAG: --leader-elect-retry-period="2s"
I0208 12:29:27.149741       1 flags.go:52] FLAG: --log-backtrace-at=":0"
I0208 12:29:27.149748       1 flags.go:52] FLAG: --log-dir=""
I0208 12:29:27.149753       1 flags.go:52] FLAG: --log-file=""
I0208 12:29:27.149756       1 flags.go:52] FLAG: --log-file-max-size="1800"
I0208 12:29:27.149761       1 flags.go:52] FLAG: --logtostderr="true"
I0208 12:29:27.149765       1 flags.go:52] FLAG: --max-autoprovisioned-node-group-count="15"
I0208 12:29:27.149770       1 flags.go:52] FLAG: --max-bulk-soft-taint-count="10"
I0208 12:29:27.149775       1 flags.go:52] FLAG: --max-bulk-soft-taint-time="3s"
I0208 12:29:27.149779       1 flags.go:52] FLAG: --max-empty-bulk-delete="10"
I0208 12:29:27.149800       1 flags.go:52] FLAG: --max-failing-time="15m0s"
I0208 12:29:27.149813       1 flags.go:52] FLAG: --max-graceful-termination-sec="600"
I0208 12:29:27.149818       1 flags.go:52] FLAG: --max-inactivity="10m0s"
I0208 12:29:27.149822       1 flags.go:52] FLAG: --max-node-provision-time="15m0s"
I0208 12:29:27.149838       1 flags.go:52] FLAG: --max-nodes-total="0"
I0208 12:29:27.149842       1 flags.go:52] FLAG: --max-total-unready-percentage="45"
I0208 12:29:27.149848       1 flags.go:52] FLAG: --memory-total="0:6400000"
I0208 12:29:27.149852       1 flags.go:52] FLAG: --min-replica-count="0"
I0208 12:29:27.149856       1 flags.go:52] FLAG: --namespace="kube-system"
I0208 12:29:27.149861       1 flags.go:52] FLAG: --new-pod-scale-up-delay="0s"
I0208 12:29:27.149865       1 flags.go:52] FLAG: --node-autoprovisioning-enabled="false"
I0208 12:29:27.149870       1 flags.go:52] FLAG: --node-deletion-delay-timeout="2m0s"
I0208 12:29:27.149874       1 flags.go:52] FLAG: --node-group-auto-discovery="[asg:tag=k8s.io/cluster-autoscaler/enabled,k8s.io/cluster-autoscaler/udacity-cluster]"
I0208 12:29:27.149887       1 flags.go:52] FLAG: --nodes="[]"
I0208 12:29:27.149892       1 flags.go:52] FLAG: --ok-total-unready-count="3"
I0208 12:29:27.149897       1 flags.go:52] FLAG: --one-output="false"
I0208 12:29:27.149920       1 flags.go:52] FLAG: --profiling="false"
I0208 12:29:27.149925       1 flags.go:52] FLAG: --regional="false"
I0208 12:29:27.149930       1 flags.go:52] FLAG: --scale-down-candidates-pool-min-count="50"
I0208 12:29:27.149934       1 flags.go:52] FLAG: --scale-down-candidates-pool-ratio="0.1"
I0208 12:29:27.149940       1 flags.go:52] FLAG: --scale-down-delay-after-add="10m0s"
I0208 12:29:27.149951       1 flags.go:52] FLAG: --scale-down-delay-after-delete="0s"
I0208 12:29:27.149955       1 flags.go:52] FLAG: --scale-down-delay-after-failure="3m0s"
I0208 12:29:27.149960       1 flags.go:52] FLAG: --scale-down-enabled="true"
I0208 12:29:27.149965       1 flags.go:52] FLAG: --scale-down-gpu-utilization-threshold="0.5"
I0208 12:29:27.149970       1 flags.go:52] FLAG: --scale-down-non-empty-candidates-count="30"
I0208 12:29:27.149974       1 flags.go:52] FLAG: --scale-down-unneeded-time="10m0s"
I0208 12:29:27.149993       1 flags.go:52] FLAG: --scale-down-unready-time="20m0s"
I0208 12:29:27.150000       1 flags.go:52] FLAG: --scale-down-utilization-threshold="0.5"
I0208 12:29:27.150004       1 flags.go:52] FLAG: --scale-up-from-zero="true"
I0208 12:29:27.150009       1 flags.go:52] FLAG: --scan-interval="10s"
I0208 12:29:27.150014       1 flags.go:52] FLAG: --skip-headers="false"
I0208 12:29:27.150027       1 flags.go:52] FLAG: --skip-log-headers="false"
I0208 12:29:27.150033       1 flags.go:52] FLAG: --skip-nodes-with-local-storage="false"
I0208 12:29:27.150038       1 flags.go:52] FLAG: --skip-nodes-with-system-pods="false"
I0208 12:29:27.150042       1 flags.go:52] FLAG: --status-config-map-name="cluster-autoscaler-status"
I0208 12:29:27.150048       1 flags.go:52] FLAG: --stderrthreshold="0"
I0208 12:29:27.150052       1 flags.go:52] FLAG: --unremovable-node-recheck-timeout="5m0s"
I0208 12:29:27.150058       1 flags.go:52] FLAG: --user-agent="cluster-autoscaler"
I0208 12:29:27.150063       1 flags.go:52] FLAG: --v="4"
I0208 12:29:27.150071       1 flags.go:52] FLAG: --vmodule=""
I0208 12:29:27.150076       1 flags.go:52] FLAG: --write-status-configmap="true"
I0208 12:29:27.150084       1 main.go:391] Cluster Autoscaler 1.21.0
I0208 12:29:27.268757       1 leaderelection.go:243] attempting to acquire leader lease kube-system/cluster-autoscaler...
I0208 12:29:27.284630       1 leaderelection.go:253] successfully acquired lease kube-system/cluster-autoscaler
I0208 12:29:27.284958       1 event_sink_logging_wrapper.go:48] Event(v1.ObjectReference{Kind:"Lease", Namespace:"kube-system", Name:"cluster-autoscaler", UID:"7a13ed30-3c15-4712-b78f-093f0cac366c", APIVersion:"coordination.k8s.io/v1", ResourceVersion:"23114", FieldPath:""}): type: 'Normal' reason: 'LeaderElection' cluster-autoscaler-d8cc67649-95ml7 became leader
I0208 12:29:27.286965       1 reflector.go:219] Starting reflector *v1.Pod (1h0m0s) from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:188
I0208 12:29:27.286986       1 reflector.go:255] Listing and watching *v1.Pod from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:188
I0208 12:29:27.287206       1 reflector.go:219] Starting reflector *v1.Pod (1h0m0s) from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:212
I0208 12:29:27.287221       1 reflector.go:255] Listing and watching *v1.Pod from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:212
I0208 12:29:27.287411       1 reflector.go:219] Starting reflector *v1.Node (1h0m0s) from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:246
I0208 12:29:27.287424       1 reflector.go:255] Listing and watching *v1.Node from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:246
I0208 12:29:27.287571       1 reflector.go:219] Starting reflector *v1.Node (1h0m0s) from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:246
I0208 12:29:27.287584       1 reflector.go:255] Listing and watching *v1.Node from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:246
I0208 12:29:27.287740       1 reflector.go:219] Starting reflector *v1beta1.PodDisruptionBudget (1h0m0s) from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:309
I0208 12:29:27.287751       1 reflector.go:255] Listing and watching *v1beta1.PodDisruptionBudget from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:309
I0208 12:29:27.287917       1 reflector.go:219] Starting reflector *v1.DaemonSet (1h0m0s) from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:320
I0208 12:29:27.287934       1 reflector.go:255] Listing and watching *v1.DaemonSet from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:320
I0208 12:29:27.288091       1 reflector.go:219] Starting reflector *v1.ReplicationController (1h0m0s) from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:329
I0208 12:29:27.288104       1 reflector.go:255] Listing and watching *v1.ReplicationController from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:329
I0208 12:29:27.288255       1 reflector.go:219] Starting reflector *v1.Job (1h0m0s) from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:338
I0208 12:29:27.288267       1 reflector.go:255] Listing and watching *v1.Job from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:338
I0208 12:29:27.288452       1 reflector.go:219] Starting reflector *v1.ReplicaSet (1h0m0s) from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:347
I0208 12:29:27.288466       1 reflector.go:255] Listing and watching *v1.ReplicaSet from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:347
I0208 12:29:27.288611       1 reflector.go:219] Starting reflector *v1.StatefulSet (1h0m0s) from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:356
I0208 12:29:27.288624       1 reflector.go:255] Listing and watching *v1.StatefulSet from k8s.io/autoscaler/cluster-autoscaler/utils/kubernetes/listers.go:356
W0208 12:29:27.551706       1 warnings.go:70] policy/v1beta1 PodDisruptionBudget is deprecated in v1.21+, unavailable in v1.25+; use policy/v1 PodDisruptionBudget
I0208 12:29:27.851430       1 cloud_provider_builder.go:29] Building aws cloud provider.
I0208 12:29:27.851799       1 aws_util.go:76] fetching https://pricing.us-east-1.amazonaws.com/offers/v1.0/aws/AmazonEC2/current/us-east-2/index.json
W0208 12:29:27.953354       1 warnings.go:70] policy/v1beta1 PodDisruptionBudget is deprecated in v1.21+, unavailable in v1.25+; use policy/v1 PodDisruptionBudget
I0208 12:29:28.149039       1 reflector.go:219] Starting reflector *v1.ReplicationController (0s) from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.149076       1 reflector.go:255] Listing and watching *v1.ReplicationController from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.149588       1 reflector.go:219] Starting reflector *v1.StatefulSet (0s) from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.149755       1 reflector.go:255] Listing and watching *v1.StatefulSet from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.149963       1 reflector.go:219] Starting reflector *v1.PersistentVolumeClaim (0s) from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.149979       1 reflector.go:255] Listing and watching *v1.PersistentVolumeClaim from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.150282       1 reflector.go:219] Starting reflector *v1.CSIDriver (0s) from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.150448       1 reflector.go:255] Listing and watching *v1.CSIDriver from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.150523       1 reflector.go:219] Starting reflector *v1beta1.CSIStorageCapacity (0s) from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.150562       1 reflector.go:255] Listing and watching *v1beta1.CSIStorageCapacity from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.150933       1 reflector.go:219] Starting reflector *v1.PodDisruptionBudget (0s) from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.151083       1 reflector.go:255] Listing and watching *v1.PodDisruptionBudget from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.151641       1 reflector.go:219] Starting reflector *v1.ReplicaSet (0s) from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.152556       1 reflector.go:255] Listing and watching *v1.ReplicaSet from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.151087       1 reflector.go:219] Starting reflector *v1.Service (0s) from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.250109       1 reflector.go:255] Listing and watching *v1.Service from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.150386       1 reflector.go:219] Starting reflector *v1.PersistentVolume (0s) from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.250568       1 reflector.go:255] Listing and watching *v1.PersistentVolume from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.149823       1 reflector.go:219] Starting reflector *v1.Node (0s) from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.251049       1 reflector.go:255] Listing and watching *v1.Node from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.249262       1 reflector.go:219] Starting reflector *v1.Pod (0s) from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.251504       1 reflector.go:255] Listing and watching *v1.Pod from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.249403       1 reflector.go:219] Starting reflector *v1.CSINode (0s) from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.251967       1 reflector.go:255] Listing and watching *v1.CSINode from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.149665       1 reflector.go:219] Starting reflector *v1.StorageClass (0s) from k8s.io/client-go/informers/factory.go:134
I0208 12:29:28.252543       1 reflector.go:255] Listing and watching *v1.StorageClass from k8s.io/client-go/informers/factory.go:134
I0208 12:30:00.260196       1 aws_cloud_provider.go:374] Using static instance type f1.4xlarge
I0208 12:30:00.260226       1 aws_cloud_provider.go:374] Using static instance type m1.large
I0208 12:30:00.260231       1 aws_cloud_provider.go:374] Using static instance type m3.xlarge
I0208 12:30:00.260237       1 aws_cloud_provider.go:374] Using static instance type c3.2xlarge
I0208 12:30:00.260242       1 aws_cloud_provider.go:374] Using static instance type d3en.xlarge
I0208 12:30:00.260249       1 aws_cloud_provider.go:374] Using static instance type hs1.8xlarge
I0208 12:30:00.260256       1 aws_cloud_provider.go:374] Using static instance type c1.xlarge
I0208 12:30:00.260262       1 aws_cloud_provider.go:374] Using static instance type d3en.2xlarge
I0208 12:30:00.260270       1 aws_cloud_provider.go:374] Using static instance type u-9tb1.metal
I0208 12:30:00.260277       1 aws_cloud_provider.go:374] Using static instance type u-9tb1
I0208 12:30:00.260282       1 aws_cloud_provider.go:374] Using static instance type d3en.4xlarge
I0208 12:30:00.260294       1 aws_cloud_provider.go:374] Using static instance type c3.4xlarge
I0208 12:30:00.260305       1 aws_cloud_provider.go:374] Using static instance type m1.xlarge
I0208 12:30:00.260312       1 aws_cloud_provider.go:374] Using static instance type d3en.12xlarge
I0208 12:30:00.260318       1 aws_cloud_provider.go:374] Using static instance type m2.4xlarge
I0208 12:30:00.260325       1 aws_cloud_provider.go:374] Using static instance type c3
I0208 12:30:00.260334       1 aws_cloud_provider.go:374] Using static instance type g2
I0208 12:30:00.260352       1 aws_cloud_provider.go:374] Using static instance type c3.xlarge
I0208 12:30:00.349167       1 aws_cloud_provider.go:374] Using static instance type f1.16xlarge
I0208 12:30:00.349197       1 aws_cloud_provider.go:374] Using static instance type m1.medium
I0208 12:30:00.349204       1 aws_cloud_provider.go:374] Using static instance type g2.2xlarge
I0208 12:30:00.349221       1 aws_cloud_provider.go:374] Using static instance type m3.medium
I0208 12:30:00.349228       1 aws_cloud_provider.go:374] Using static instance type p3dn.24xlarge
I0208 12:30:00.349254       1 aws_cloud_provider.go:374] Using static instance type p3dn
I0208 12:30:00.349269       1 aws_cloud_provider.go:374] Using static instance type m3.2xlarge
I0208 12:30:00.349278       1 aws_cloud_provider.go:374] Using static instance type d3en.8xlarge
I0208 12:30:00.349298       1 aws_cloud_provider.go:374] Using static instance type cr1.8xlarge
I0208 12:30:00.349335       1 aws_cloud_provider.go:374] Using static instance type m2.xlarge
I0208 12:30:00.349345       1 aws_cloud_provider.go:374] Using static instance type u-18tb1.metal
I0208 12:30:00.349352       1 aws_cloud_provider.go:374] Using static instance type f1.2xlarge
I0208 12:30:00.349359       1 aws_cloud_provider.go:374] Using static instance type c1.medium
I0208 12:30:00.349368       1 aws_cloud_provider.go:374] Using static instance type i3p.16xlarge
I0208 12:30:00.349374       1 aws_cloud_provider.go:374] Using static instance type m3
I0208 12:30:00.349455       1 aws_cloud_provider.go:374] Using static instance type t1.micro
I0208 12:30:00.349482       1 aws_cloud_provider.go:374] Using static instance type c3.8xlarge
I0208 12:30:00.349487       1 aws_cloud_provider.go:374] Using static instance type f1
I0208 12:30:00.349492       1 aws_cloud_provider.go:374] Using static instance type g2.8xlarge
I0208 12:30:00.350077       1 aws_cloud_provider.go:374] Using static instance type m2.2xlarge
I0208 12:30:00.350108       1 aws_cloud_provider.go:374] Using static instance type m3.large
I0208 12:30:00.350133       1 aws_cloud_provider.go:374] Using static instance type u-24tb1
I0208 12:30:00.350144       1 aws_cloud_provider.go:374] Using static instance type cc2.8xlarge
I0208 12:30:00.350154       1 aws_cloud_provider.go:374] Using static instance type c3.large
I0208 12:30:00.350161       1 aws_cloud_provider.go:374] Using static instance type m1.small
I0208 12:30:00.350174       1 aws_cloud_provider.go:374] Using static instance type u-24tb1.metal
I0208 12:30:00.350179       1 aws_cloud_provider.go:374] Using static instance type d3en.6xlarge
I0208 12:30:00.350217       1 aws_cloud_provider.go:374] Using static instance type u-18tb1
I0208 12:30:00.350254       1 aws_cloud_provider.go:384] Successfully load 662 EC2 Instance Types [g3.16xlarge g2.2xlarge c3.2xlarge f1 r6a.2xlarge c6a.48xlarge t3.medium r5n.8xlarge x2gd.large m6i.metal m5n.metal r5dn.24xlarge r6i.12xlarge x2iedn.4xlarge x2gd.medium c3 t1.micro r6gd.2xlarge m6a.metal t3.large i3en.6xlarge x1e.xlarge h1.2xlarge r3.large c5n.metal is4gen.4xlarge r6a.large m5d.8xlarge m5.24xlarge g4dn.12xlarge z1d.12xlarge h1.4xlarge d3en.12xlarge r6g c6gd.4xlarge r5d.12xlarge u-12tb1.metal g3.8xlarge m5zn.12xlarge r5b x2gd.12xlarge r6id i3p.16xlarge c7g x2iedn.16xlarge m5zn.2xlarge c6a.2xlarge m6idn.12xlarge i4i.16xlarge c6gd.12xlarge c6gn.4xlarge x2iedn.2xlarge r5n.4xlarge m5dn u-24tb1 r5dn.8xlarge m5ad.4xlarge g4dn.16xlarge t3.micro c5a.xlarge z1d.metal c6gd m1.large c4.xlarge r5.16xlarge m5.4xlarge r6id.large r5d.4xlarge m5.8xlarge m6i.8xlarge r6i.32xlarge d3.8xlarge r6in.24xlarge m5a.large x1e.2xlarge x1.16xlarge a1.metal r6idn.24xlarge c6g.medium r6a.24xlarge u-3tb1.56xlarge c5a.2xlarge c5ad.12xlarge c6id.24xlarge is4gen.2xlarge m6i.2xlarge c7g.8xlarge a1.2xlarge m6g a1 m5ad.xlarge i4i.8xlarge c6a.12xlarge x2gd.metal m5ad.large c6in.large c5.2xlarge i3 r6id.16xlarge r6gd c6g.large r6i m6a.12xlarge c6in.12xlarge p4d.24xlarge c5d.2xlarge z1d.2xlarge r5ad.xlarge g4dn.4xlarge h1.16xlarge r4.2xlarge m5n.xlarge m5.xlarge m5dn.large d2.2xlarge c5ad.8xlarge m6id.4xlarge i3.16xlarge m6a.large c5n.large r6id.4xlarge g4ad.8xlarge m6id m4.16xlarge c4.large c6gn.12xlarge d3.4xlarge u-6tb1.metal c6gd.medium c6gd.16xlarge m6idn.large c5a.12xlarge c5n.4xlarge m5n.12xlarge d3.2xlarge x1 x2gd.2xlarge m6idn.xlarge g4ad.16xlarge c7g.2xlarge m5dn.12xlarge r4.16xlarge r6id.2xlarge r5dn.large mac2 r4.large c6a.large c6a.4xlarge m6g.8xlarge inf1 r5ad.16xlarge m5a.12xlarge r6g.8xlarge r5dn.2xlarge r6gd.12xlarge c6g.metal inf1.6xlarge r6id.8xlarge g2.8xlarge m6i.12xlarge r5a.16xlarge c5.metal i3.4xlarge r5n.16xlarge c5ad.xlarge c6id m6gd.8xlarge r5b.2xlarge t2.2xlarge c6in.4xlarge r6a.4xlarge m5zn.large t3a.nano i3en m5n.16xlarge x1e.32xlarge r6gd.metal i3en.xlarge c6gn.16xlarge t4g.2xlarge t4g.medium c6gn.8xlarge r6gd.4xlarge im4gn.xlarge c5ad.large c5d.18xlarge u-9tb1 m6id.large inf1.2xlarge r5dn.4xlarge c5n.9xlarge t3a.small f1.4xlarge u-9tb1.metal r6gd.medium c5.xlarge r5d.xlarge c6gd.large m5n c6in.16xlarge r5b.12xlarge p3.2xlarge m5.metal t2.xlarge c5n.xlarge c5.18xlarge r5a.xlarge c6gd.xlarge m5zn.xlarge r6id.xlarge c3.4xlarge u-24tb1.metal u-12tb1.112xlarge g3s.xlarge c6g m1.small c6i.xlarge c5n.18xlarge m5d.large c6g.4xlarge r6in.xlarge m6i.xlarge r5dn p3dn m5ad.8xlarge r5n.12xlarge r6gd.large c5n t3 m4 g3 m6idn.2xlarge r4.8xlarge m6a.xlarge m5d.12xlarge m5zn t4g.nano c6gd.8xlarge c4 m5n.4xlarge z1d.large r6in.16xlarge i3en.2xlarge c5.large m1.xlarge c5d.12xlarge r5n.metal m6gd.12xlarge r6idn.32xlarge c6i.4xlarge m5a.8xlarge c4.2xlarge c3.xlarge m5ad.24xlarge r5d.metal r6g.16xlarge m3.medium c6a.metal x2idn.metal r6a.48xlarge t3.small r4 x2gd.16xlarge r6gd.8xlarge r5.4xlarge r5d.8xlarge r5b.16xlarge r5b.4xlarge r6a.32xlarge c6a.16xlarge c7g.4xlarge x2idn.24xlarge x2idn.16xlarge a1.large r5a.12xlarge m5d.4xlarge c6i.8xlarge c5a.8xlarge a1.xlarge m5dn.2xlarge m5dn.4xlarge m5n.2xlarge h1 c1.medium m5d.2xlarge m6i.large r6a mac1 p3dn.24xlarge x1e.16xlarge c5.9xlarge c6g.xlarge i3.large m5.2xlarge m5a.2xlarge m4.4xlarge c6id.metal g4dn.2xlarge c5d m5d r6gd.xlarge c5ad.16xlarge c6a.xlarge c5a.large r6a.16xlarge m6in.12xlarge c6id.12xlarge c6g.2xlarge c6i.32xlarge m6in.xlarge x2iedn.metal m5d.24xlarge g4ad r6i.2xlarge d3en.2xlarge p2.xlarge i3.2xlarge x1e d3en.4xlarge m6g.large im4gn.2xlarge u-18tb1.metal m6in.16xlarge m6id.16xlarge c6in.8xlarge z1d.6xlarge c5d.large r5ad.24xlarge r6i.24xlarge g4dn m5ad.16xlarge c6g.12xlarge t2.medium m5d.xlarge a1.4xlarge r3.8xlarge c6gn.large c5ad.4xlarge is4gen.large p4d m6i.4xlarge c5ad.2xlarge c6in.32xlarge x2iedn i3.metal x2gd.8xlarge d3en.8xlarge m2.2xlarge i3.xlarge r6g.2xlarge m6gd.16xlarge r6id.metal i2 c6id.xlarge c5n.2xlarge r6gd.16xlarge c6i.16xlarge c4.8xlarge z1d.3xlarge m6a.32xlarge r5n.xlarge c6id.16xlarge i4i.metal c5a.4xlarge r6in.32xlarge x2iedn.24xlarge r5.metal x2gd.4xlarge c7g.metal a1.medium c6gn.2xlarge c6id.2xlarge m6i.16xlarge hpc6id.32xlarge i4i.32xlarge t4g.xlarge r5d m6gd m6a.16xlarge m6in.2xlarge i3en.large m5n.8xlarge x2iedn.xlarge im4gn.16xlarge m6in m5zn.3xlarge r5n.24xlarge r6in.large m5.16xlarge r5dn.12xlarge m4.10xlarge x2gd d2 c7g.xlarge r5n.2xlarge m6idn.32xlarge h1.8xlarge i3en.3xlarge hs1.8xlarge c6i.metal c6i.2xlarge m6id.12xlarge m6in.8xlarge r5a.24xlarge m4.large r5d.2xlarge c5.4xlarge m6idn.8xlarge g4ad.4xlarge r6a.xlarge i3en.24xlarge t3a.micro x2gd.xlarge r6idn.4xlarge x2iedn.8xlarge c6gd.metal r6in.8xlarge r6i.8xlarge r5ad.2xlarge m6g.4xlarge cr1.8xlarge i4i.large i4i.4xlarge r5.12xlarge r6idn.12xlarge im4gn.4xlarge r3.2xlarge t2.micro m5ad.2xlarge m4.xlarge r6i.4xlarge c5d.9xlarge m5n.24xlarge is4gen.medium p2 g2 c5d.xlarge r6i.large c6id.8xlarge r6in.12xlarge im4gn.8xlarge t2.large c3.large m6g.xlarge z1d.xlarge r5d.large m5dn.16xlarge m6g.metal r5dn.metal r4.4xlarge r5ad.4xlarge c6gn.xlarge r6idn.xlarge m5dn.metal m5d.metal hpc6a.48xlarge g4dn.metal m6idn.4xlarge t4g.large m1.medium c6a.8xlarge c5d.24xlarge m6id.32xlarge r6g.metal c6a.24xlarge r5b.metal m6idn.24xlarge r6g.medium g4dn.xlarge m5n.large im4gn.large c6g.16xlarge t2.nano r6a.12xlarge m6a.24xlarge r5.large m6i.24xlarge g4ad.xlarge d3en.xlarge r5a.2xlarge m5a.4xlarge m6idn r5.24xlarge m5ad.12xlarge u-6tb1 c5ad.24xlarge m6g.2xlarge c6in.2xlarge i4i c5.12xlarge m6gd.large i3.8xlarge c6id.4xlarge r6g.12xlarge m5.large t4g.small m6i c6in.24xlarge m3.xlarge i2.large mac1.metal m6a d2.8xlarge r6i.xlarge m6id.8xlarge r5n.large i2.8xlarge m2.xlarge c6in.xlarge u-18tb1 f1.16xlarge c3.8xlarge r5ad.12xlarge g4dn.8xlarge t3a.2xlarge inf1.24xlarge r6in m6a.8xlarge r6id.24xlarge r6id.32xlarge m6g.16xlarge x1e.8xlarge c5.24xlarge t3a.xlarge r5.2xlarge i3en.metal r5b.xlarge u-12tb1 im4gn r5 m2.4xlarge m3.2xlarge r5dn.16xlarge m6gd.4xlarge m5dn.8xlarge x1e.4xlarge m6gd.xlarge r5a.8xlarge mac2.metal m5 d2.xlarge m6id.2xlarge t3.xlarge c5d.4xlarge m6idn.16xlarge r5.8xlarge m5.12xlarge r5a.4xlarge m5d.16xlarge r6a.8xlarge t3a.large r6idn.large r5b.24xlarge m6a.48xlarge u-6tb1.56xlarge is4gen.xlarge r3 i2.2xlarge c5d.metal c6gn.medium c7g.medium p2.8xlarge p3.16xlarge i4i.xlarge u-6tb1.112xlarge c5a.16xlarge r5ad.large c7g.12xlarge is4gen.8xlarge m6g.12xlarge r5b.8xlarge z1d x2idn cc2.8xlarge r6idn.16xlarge r5b.large m5zn.metal t3.nano r6g.4xlarge r5n i4i.2xlarge r5d.16xlarge p3.8xlarge m6in.32xlarge c6a.32xlarge r6a.metal c6gd.2xlarge c6i.24xlarge r3.xlarge c5a.24xlarge x1.32xlarge m4.2xlarge m6a.2xlarge m6gd.2xlarge r5.xlarge t4g.micro g4ad.2xlarge r6id.12xlarge r6in.2xlarge c6i.12xlarge r6i.16xlarge r4.xlarge r6idn.2xlarge t2.small m6id.xlarge c4.4xlarge m6g.medium c1.xlarge f1.2xlarge m5zn.6xlarge x2idn.32xlarge m6in.24xlarge p2.16xlarge m6in.4xlarge m3.large m6i.32xlarge c6i.large r5d.24xlarge r5a.large m5dn.xlarge d3.xlarge c7g.16xlarge r6g.xlarge m3 i2.4xlarge m5a.16xlarge m5a.24xlarge r6idn c6gn r6g.large c6g.8xlarge r5ad.8xlarge i3en.12xlarge m5a.xlarge c6id.32xlarge t3a.medium c6in d3en.6xlarge r3.4xlarge x2iedn.32xlarge m6in.large m6gd.medium r6idn.8xlarge c7g.large r6in.4xlarge r5dn.xlarge c6id.large r6i.metal g3.4xlarge p3 c5 m6id.metal m6a.4xlarge m6id.24xlarge d2.4xlarge m6gd.metal inf1.xlarge t3.2xlarge c6i c6a m5dn.24xlarge i2.xlarge]
I0208 12:30:00.778512       1 auto_scaling_groups.go:351] Regenerating instance to ASG map for ASGs: [eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f]
I0208 12:30:00.850685       1 auto_scaling.go:199] 1 launch configurations already in cache
I0208 12:30:00.850718       1 auto_scaling_groups.go:136] Registering ASG eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f
I0208 12:30:00.850728       1 aws_manager.go:269] Refreshed ASG list, next refresh after 2023-02-08 12:31:00.85072494 +0000 UTC m=+94.301540266
I0208 12:30:00.850882       1 main.go:291] Registered cleanup signal handler
I0208 12:30:00.850912       1 node_instances_cache.go:156] Start refreshing cloud provider node instances cache
I0208 12:30:00.850929       1 node_instances_cache.go:168] Refresh cloud provider node instances cache finished, refresh took 5.212µs
I0208 12:30:10.851051       1 static_autoscaler.go:228] Starting main loop
W0208 12:30:10.851400       1 clusterstate.go:432] AcceptableRanges have not been populated yet. Skip checking
I0208 12:30:10.851531       1 filter_out_schedulable.go:65] Filtering out schedulables
I0208 12:30:10.851564       1 filter_out_schedulable.go:132] Filtered out 0 pods using hints
I0208 12:30:10.851575       1 filter_out_schedulable.go:170] 0 pods were kept as unschedulable based on caching
I0208 12:30:10.851581       1 filter_out_schedulable.go:171] 0 pods marked as unschedulable can be scheduled.
I0208 12:30:10.851614       1 filter_out_schedulable.go:82] No schedulable pods
I0208 12:30:10.851641       1 static_autoscaler.go:401] No unschedulable pods
I0208 12:30:10.851659       1 static_autoscaler.go:448] Calculating unneeded nodes
I0208 12:30:10.851698       1 scale_down.go:447] Node ip-10-100-3-81.us-east-2.compute.internal - cpu utilization 0.168394
I0208 12:30:10.851752       1 scale_down.go:447] Node ip-10-100-1-233.us-east-2.compute.internal - memory utilization 0.150165
I0208 12:30:10.851782       1 scale_down.go:564] Finding additional 2 candidates for scale down.
I0208 12:30:10.851792       1 cluster.go:148] Fast evaluation: ip-10-100-3-81.us-east-2.compute.internal for removal
I0208 12:30:10.852297       1 cluster.go:360] Pod kube-system/coredns-f47955f89-6fdjx can be moved to ip-10-100-1-233.us-east-2.compute.internal
I0208 12:30:10.852381       1 cluster.go:360] Pod kube-system/coredns-f47955f89-kzrk4 can be moved to ip-10-100-1-233.us-east-2.compute.internal
I0208 12:30:10.852407       1 cluster.go:187] Fast evaluation: node ip-10-100-3-81.us-east-2.compute.internal may be removed
I0208 12:30:10.852417       1 cluster.go:148] Fast evaluation: ip-10-100-1-233.us-east-2.compute.internal for removal
I0208 12:30:10.852453       1 cluster.go:360] Pod kube-system/cluster-autoscaler-d8cc67649-95ml7 can be moved to ip-10-100-3-81.us-east-2.compute.internal
I0208 12:30:10.852469       1 cluster.go:187] Fast evaluation: node ip-10-100-1-233.us-east-2.compute.internal may be removed
I0208 12:30:10.852534       1 static_autoscaler.go:491] ip-10-100-3-81.us-east-2.compute.internal is unneeded since 2023-02-08 12:30:10.851005311 +0000 UTC m=+44.301820647 duration 0s
I0208 12:30:10.852551       1 static_autoscaler.go:491] ip-10-100-1-233.us-east-2.compute.internal is unneeded since 2023-02-08 12:30:10.851005311 +0000 UTC m=+44.301820647 duration 0s
I0208 12:30:10.852565       1 static_autoscaler.go:502] Scale down status: unneededOnly=true lastScaleUpTime=2023-02-08 12:30:00.850750828 +0000 UTC m=+34.301566142 lastScaleDownDeleteTime=2023-02-08 12:30:00.850750888 +0000 UTC m=+34.301566200 lastScaleDownFailTime=2023-02-08 12:30:00.850750946 +0000 UTC m=+34.301566258 scaleDownForbidden=false isDeleteInProgress=false scaleDownInCooldown=true
I0208 12:30:20.872913       1 static_autoscaler.go:228] Starting main loop
I0208 12:30:20.873278       1 filter_out_schedulable.go:65] Filtering out schedulables
I0208 12:30:20.873296       1 filter_out_schedulable.go:132] Filtered out 0 pods using hints
I0208 12:30:20.873305       1 filter_out_schedulable.go:170] 0 pods were kept as unschedulable based on caching
I0208 12:30:20.873311       1 filter_out_schedulable.go:171] 0 pods marked as unschedulable can be scheduled.
I0208 12:30:20.873320       1 filter_out_schedulable.go:82] No schedulable pods
I0208 12:30:20.873367       1 static_autoscaler.go:401] No unschedulable pods
I0208 12:30:20.873388       1 static_autoscaler.go:448] Calculating unneeded nodes
I0208 12:30:20.873428       1 scale_down.go:447] Node ip-10-100-1-233.us-east-2.compute.internal - memory utilization 0.150165
I0208 12:30:20.873459       1 scale_down.go:447] Node ip-10-100-3-81.us-east-2.compute.internal - cpu utilization 0.168394
I0208 12:30:20.873485       1 cluster.go:148] Fast evaluation: ip-10-100-1-233.us-east-2.compute.internal for removal
I0208 12:30:20.873545       1 cluster.go:345] Pod kube-system/cluster-autoscaler-d8cc67649-95ml7 can be moved to ip-10-100-3-81.us-east-2.compute.internal
I0208 12:30:20.873563       1 cluster.go:187] Fast evaluation: node ip-10-100-1-233.us-east-2.compute.internal may be removed
I0208 12:30:20.873572       1 cluster.go:148] Fast evaluation: ip-10-100-3-81.us-east-2.compute.internal for removal
I0208 12:30:20.873635       1 cluster.go:345] Pod kube-system/coredns-f47955f89-6fdjx can be moved to ip-10-100-1-233.us-east-2.compute.internal
I0208 12:30:20.873688       1 cluster.go:345] Pod kube-system/coredns-f47955f89-kzrk4 can be moved to ip-10-100-1-233.us-east-2.compute.internal
I0208 12:30:20.873708       1 cluster.go:187] Fast evaluation: node ip-10-100-3-81.us-east-2.compute.internal may be removed
I0208 12:30:20.873749       1 static_autoscaler.go:491] ip-10-100-1-233.us-east-2.compute.internal is unneeded since 2023-02-08 12:30:10.851005311 +0000 UTC m=+44.301820647 duration 10.021882408s
I0208 12:30:20.873771       1 static_autoscaler.go:491] ip-10-100-3-81.us-east-2.compute.internal is unneeded since 2023-02-08 12:30:10.851005311 +0000 UTC m=+44.301820647 duration 10.021882408s
I0208 12:30:20.873792       1 static_autoscaler.go:502] Scale down status: unneededOnly=true lastScaleUpTime=2023-02-08 12:30:00.850750828 +0000 UTC m=+34.301566142 lastScaleDownDeleteTime=2023-02-08 12:30:00.850750888 +0000 UTC m=+34.301566200 lastScaleDownFailTime=2023-02-08 12:30:00.850750946 +0000 UTC m=+34.301566258 scaleDownForbidden=false isDeleteInProgress=false scaleDownInCooldown=true
```

## Redeploy bloatware

```bash
Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl apply -f starter/apps/bloatware/bloatware.yml
deployment.apps/bloaty-mcbloatface created
```

## Logs of autoscaler

```bash
Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl -n kube-system logs -f deployment/cluster-autoscaler
I0208 13:02:01.402028       1 static_autoscaler.go:228] Starting main loop
W0208 13:02:01.402476       1 clusterstate.go:432] AcceptableRanges have not been populated yet. Skip checking
I0208 13:02:01.402605       1 filter_out_schedulable.go:65] Filtering out schedulables
I0208 13:02:01.402645       1 filter_out_schedulable.go:132] Filtered out 0 pods using hints
I0208 13:02:01.402825       1 filter_out_schedulable.go:170] 0 pods were kept as unschedulable based on caching
I0208 13:02:01.402838       1 filter_out_schedulable.go:171] 0 pods marked as unschedulable can be scheduled.
I0208 13:02:01.402892       1 filter_out_schedulable.go:82] No schedulable pods
I0208 13:02:01.402914       1 klogx.go:86] Pod udacity/bloaty-mcbloatface-58c98b98d8-jcld5 is unschedulable
I0208 13:02:01.402921       1 klogx.go:86] Pod udacity/bloaty-mcbloatface-58c98b98d8-8kzdk is unschedulable
I0208 13:02:01.402927       1 klogx.go:86] Pod udacity/bloaty-mcbloatface-58c98b98d8-7sbjl is unschedulable
I0208 13:02:01.402949       1 klogx.go:86] Pod udacity/bloaty-mcbloatface-58c98b98d8-4fqb8 is unschedulable
I0208 13:02:01.402955       1 klogx.go:86] Pod udacity/bloaty-mcbloatface-58c98b98d8-tfvbz is unschedulable
I0208 13:02:01.402995       1 scale_up.go:376] Upcoming 0 nodes
I0208 13:02:01.403453       1 waste.go:57] Expanding Node Group eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f would waste 37.50% CPU, 74.17% Memory, 55.84% Blended
I0208 13:02:01.403480       1 scale_up.go:468] Best option to resize: eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f
I0208 13:02:01.403515       1 scale_up.go:472] Estimated 1 nodes needed in eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f
I0208 13:02:01.403540       1 scale_up.go:586] Final scale-up plan: [{eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f 2->3 (max: 10)}]
I0208 13:02:01.403563       1 scale_up.go:675] Scale-up: setting group eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f size to 3
I0208 13:02:01.403625       1 auto_scaling_groups.go:219] Setting asg eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f size to 3
I0208 13:02:01.403758       1 event_sink_logging_wrapper.go:48] Event(v1.ObjectReference{Kind:"ConfigMap", Namespace:"kube-system", Name:"cluster-autoscaler-status", UID:"dcd8d5eb-1d6b-46c8-88ff-47f8caaf7d7a", APIVersion:"v1", ResourceVersion:"33037", FieldPath:""}): type: 'Normal' reason: 'ScaledUpGroup' Scale-up: setting group eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f size to 3
I0208 13:02:01.595589       1 eventing_scale_up_processor.go:47] Skipping event processing for unschedulable pods since there is a ScaleUp attempt this loop
I0208 13:02:01.595714       1 event_sink_logging_wrapper.go:48] Event(v1.ObjectReference{Kind:"Pod", Namespace:"udacity", Name:"bloaty-mcbloatface-58c98b98d8-7sbjl", UID:"e2e32f61-98db-4cf4-bcd3-ce0928941e29", APIVersion:"v1", ResourceVersion:"32813", FieldPath:""}): type: 'Normal' reason: 'TriggeredScaleUp' pod triggered scale-up: [{eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f 2->3 (max: 10)}]
I0208 13:02:01.605653       1 event_sink_logging_wrapper.go:48] Event(v1.ObjectReference{Kind:"Pod", Namespace:"udacity", Name:"bloaty-mcbloatface-58c98b98d8-4fqb8", UID:"02986631-5792-4fe5-8d0b-1f33fec27650", APIVersion:"v1", ResourceVersion:"32804", FieldPath:""}): type: 'Normal' reason: 'TriggeredScaleUp' pod triggered scale-up: [{eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f 2->3 (max: 10)}]
I0208 13:02:01.615260       1 event_sink_logging_wrapper.go:48] Event(v1.ObjectReference{Kind:"Pod", Namespace:"udacity", Name:"bloaty-mcbloatface-58c98b98d8-tfvbz", UID:"a9308d6f-565d-4111-b7b5-0c310b47d6a2", APIVersion:"v1", ResourceVersion:"32816", FieldPath:""}): type: 'Normal' reason: 'TriggeredScaleUp' pod triggered scale-up: [{eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f 2->3 (max: 10)}]
I0208 13:02:01.622972       1 event_sink_logging_wrapper.go:48] Event(v1.ObjectReference{Kind:"Pod", Namespace:"udacity", Name:"bloaty-mcbloatface-58c98b98d8-jcld5", UID:"291a63ce-e781-4764-9765-f1ee963f5e42", APIVersion:"v1", ResourceVersion:"32808", FieldPath:""}): type: 'Normal' reason: 'TriggeredScaleUp' pod triggered scale-up: [{eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f 2->3 (max: 10)}]
I0208 13:02:01.628688       1 event_sink_logging_wrapper.go:48] Event(v1.ObjectReference{Kind:"Pod", Namespace:"udacity", Name:"bloaty-mcbloatface-58c98b98d8-8kzdk", UID:"2804fb56-1585-43b3-84e0-a4f72b3114a6", APIVersion:"v1", ResourceVersion:"32811", FieldPath:""}): type: 'Normal' reason: 'TriggeredScaleUp' pod triggered scale-up: [{eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f 2->3 (max: 10)}]
I0208 13:02:11.609676       1 static_autoscaler.go:228] Starting main loop
I0208 13:02:11.610293       1 clusterstate.go:248] Scale up in group eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f finished successfully in 10.014237213s
I0208 13:02:11.610371       1 filter_out_schedulable.go:65] Filtering out schedulables
I0208 13:02:11.610382       1 filter_out_schedulable.go:132] Filtered out 0 pods using hints
I0208 13:02:11.610577       1 filter_out_schedulable.go:170] 0 pods were kept as unschedulable based on caching
I0208 13:02:11.610591       1 filter_out_schedulable.go:171] 0 pods marked as unschedulable can be scheduled.
I0208 13:02:11.610602       1 filter_out_schedulable.go:82] No schedulable pods
I0208 13:02:11.610618       1 klogx.go:86] Pod udacity/bloaty-mcbloatface-58c98b98d8-7sbjl is unschedulable
I0208 13:02:11.610626       1 klogx.go:86] Pod udacity/bloaty-mcbloatface-58c98b98d8-4fqb8 is unschedulable
I0208 13:02:11.610648       1 klogx.go:86] Pod udacity/bloaty-mcbloatface-58c98b98d8-tfvbz is unschedulable
I0208 13:02:11.610655       1 klogx.go:86] Pod udacity/bloaty-mcbloatface-58c98b98d8-jcld5 is unschedulable
I0208 13:02:11.610661       1 klogx.go:86] Pod udacity/bloaty-mcbloatface-58c98b98d8-8kzdk is unschedulable
I0208 13:02:11.610700       1 scale_up.go:376] Upcoming 0 nodes
I0208 13:02:11.611165       1 waste.go:57] Expanding Node Group eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f would waste 37.50% CPU, 74.17% Memory, 55.84% Blended
I0208 13:02:11.611190       1 scale_up.go:468] Best option to resize: eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f
I0208 13:02:11.611200       1 scale_up.go:472] Estimated 1 nodes needed in eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f
I0208 13:02:11.611239       1 scale_up.go:586] Final scale-up plan: [{eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f 3->4 (max: 10)}]
I0208 13:02:11.611252       1 scale_up.go:675] Scale-up: setting group eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f size to 4
I0208 13:02:11.611276       1 auto_scaling_groups.go:219] Setting asg eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f size to 4
I0208 13:02:11.782527       1 eventing_scale_up_processor.go:47] Skipping event processing for unschedulable pods since there is a ScaleUp attempt this loop
I0208 13:02:21.799685       1 static_autoscaler.go:228] Starting main loop
I0208 13:02:21.800183       1 clusterstate.go:248] Scale up in group eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f finished successfully in 10.017174918s
I0208 13:02:21.800233       1 filter_out_schedulable.go:65] Filtering out schedulables
I0208 13:02:21.800248       1 filter_out_schedulable.go:132] Filtered out 0 pods using hints
I0208 13:02:21.800410       1 filter_out_schedulable.go:170] 0 pods were kept as unschedulable based on caching
I0208 13:02:21.800423       1 filter_out_schedulable.go:171] 0 pods marked as unschedulable can be scheduled.
I0208 13:02:21.800435       1 filter_out_schedulable.go:82] No schedulable pods
I0208 13:02:21.800454       1 klogx.go:86] Pod udacity/bloaty-mcbloatface-58c98b98d8-7sbjl is unschedulable
I0208 13:02:21.800461       1 klogx.go:86] Pod udacity/bloaty-mcbloatface-58c98b98d8-4fqb8 is unschedulable
I0208 13:02:21.800481       1 klogx.go:86] Pod udacity/bloaty-mcbloatface-58c98b98d8-tfvbz is unschedulable
I0208 13:02:21.800487       1 klogx.go:86] Pod udacity/bloaty-mcbloatface-58c98b98d8-jcld5 is unschedulable
I0208 13:02:21.800493       1 klogx.go:86] Pod udacity/bloaty-mcbloatface-58c98b98d8-8kzdk is unschedulable
I0208 13:02:21.800528       1 scale_up.go:376] Upcoming 0 nodes
I0208 13:02:21.800798       1 waste.go:57] Expanding Node Group eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f would waste 37.50% CPU, 74.17% Memory, 55.84% Blended
I0208 13:02:21.800826       1 scale_up.go:468] Best option to resize: eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f
I0208 13:02:21.800838       1 scale_up.go:472] Estimated 1 nodes needed in eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f
I0208 13:02:21.800859       1 scale_up.go:586] Final scale-up plan: [{eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f 4->5 (max: 10)}]
I0208 13:02:21.800874       1 scale_up.go:675] Scale-up: setting group eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f size to 5
I0208 13:02:21.800913       1 auto_scaling_groups.go:219] Setting asg eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f size to 5
I0208 13:02:21.974230       1 eventing_scale_up_processor.go:47] Skipping event processing for unschedulable pods since there is a ScaleUp attempt this loop
I0208 13:02:31.989175       1 static_autoscaler.go:228] Starting main loop
I0208 13:02:31.989779       1 filter_out_schedulable.go:65] Filtering out schedulables
I0208 13:02:31.989801       1 filter_out_schedulable.go:132] Filtered out 0 pods using hints
I0208 13:02:31.989860       1 filter_out_schedulable.go:157] Pod udacity.bloaty-mcbloatface-58c98b98d8-7sbjl marked as unschedulable can be scheduled on node template-node-for-eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f-2202916659517317514-0. Ignoring in scale up.
I0208 13:02:31.989885       1 filter_out_schedulable.go:157] Pod udacity.bloaty-mcbloatface-58c98b98d8-4fqb8 marked as unschedulable can be scheduled on node template-node-for-eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f-2202916659517317514-0. Ignoring in scale up.
I0208 13:02:31.989910       1 filter_out_schedulable.go:157] Pod udacity.bloaty-mcbloatface-58c98b98d8-tfvbz marked as unschedulable can be scheduled on node template-node-for-eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f-2202916659517317514-0. Ignoring in scale up.
I0208 13:02:31.989935       1 filter_out_schedulable.go:157] Pod udacity.bloaty-mcbloatface-58c98b98d8-jcld5 marked as unschedulable can be scheduled on node template-node-for-eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f-2202916659517317514-0. Ignoring in scale up.
I0208 13:02:31.989958       1 filter_out_schedulable.go:157] Pod udacity.bloaty-mcbloatface-58c98b98d8-8kzdk marked as unschedulable can be scheduled on node template-node-for-eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f-2202916659517317514-0. Ignoring in scale up.
I0208 13:02:31.989968       1 filter_out_schedulable.go:170] 0 pods were kept as unschedulable based on caching
I0208 13:02:31.989975       1 filter_out_schedulable.go:171] 5 pods marked as unschedulable can be scheduled.
I0208 13:02:31.989984       1 filter_out_schedulable.go:79] Schedulable pods present
I0208 13:02:31.990012       1 static_autoscaler.go:401] No unschedulable pods
I0208 13:02:31.990034       1 static_autoscaler.go:448] Calculating unneeded nodes
I0208 13:02:31.990101       1 scale_down.go:447] Node ip-10-100-1-233.us-east-2.compute.internal - cpu utilization 0.064767
I0208 13:02:31.990132       1 scale_down.go:447] Node ip-10-100-2-108.us-east-2.compute.internal - cpu utilization 0.064767
I0208 13:02:31.990155       1 scale_down.go:443] Node ip-10-100-1-149.us-east-2.compute.internal is not suitable for removal - cpu utilization too big (0.893782)
I0208 13:02:31.990181       1 scale_down.go:443] Node ip-10-100-3-92.us-east-2.compute.internal is not suitable for removal - cpu utilization too big (0.945596)
I0208 13:02:31.990292       1 static_autoscaler.go:491] ip-10-100-1-233.us-east-2.compute.internal is unneeded since 2023-02-08 13:02:31.989144946 +0000 UTC m=+93.192107693 duration 0s
I0208 13:02:31.990321       1 static_autoscaler.go:491] ip-10-100-2-108.us-east-2.compute.internal is unneeded since 2023-02-08 13:02:31.989144946 +0000 UTC m=+93.192107693 duration 0s
I0208 13:02:31.990338       1 static_autoscaler.go:502] Scale down status: unneededOnly=true lastScaleUpTime=2023-02-08 13:02:21.799658894 +0000 UTC m=+83.002621633 lastScaleDownDeleteTime=2023-02-08 13:01:51.401419765 +0000 UTC m=+52.604382495 lastScaleDownFailTime=2023-02-08 13:01:51.401419845 +0000 UTC m=+52.604382575 scaleDownForbidden=true isDeleteInProgress=false scaleDownInCooldown=true
I0208 13:02:42.009285       1 static_autoscaler.go:228] Starting main loop
I0208 13:02:42.009923       1 filter_out_schedulable.go:65] Filtering out schedulables
I0208 13:02:42.009952       1 filter_out_schedulable.go:132] Filtered out 0 pods using hints
I0208 13:02:42.010004       1 filter_out_schedulable.go:157] Pod udacity.bloaty-mcbloatface-58c98b98d8-7sbjl marked as unschedulable can be scheduled on node template-node-for-eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f-5793183108815074904-0. Ignoring in scale up.
I0208 13:02:42.010027       1 filter_out_schedulable.go:157] Pod udacity.bloaty-mcbloatface-58c98b98d8-4fqb8 marked as unschedulable can be scheduled on node template-node-for-eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f-5793183108815074904-0. Ignoring in scale up.
I0208 13:02:42.010056       1 filter_out_schedulable.go:157] Pod udacity.bloaty-mcbloatface-58c98b98d8-tfvbz marked as unschedulable can be scheduled on node template-node-for-eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f-5793183108815074904-0. Ignoring in scale up.
I0208 13:02:42.010075       1 filter_out_schedulable.go:157] Pod udacity.bloaty-mcbloatface-58c98b98d8-jcld5 marked as unschedulable can be scheduled on node template-node-for-eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f-5793183108815074904-0. Ignoring in scale up.
I0208 13:02:42.010101       1 filter_out_schedulable.go:157] Pod udacity.bloaty-mcbloatface-58c98b98d8-8kzdk marked as unschedulable can be scheduled on node template-node-for-eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f-5793183108815074904-0. Ignoring in scale up.
I0208 13:02:42.010110       1 filter_out_schedulable.go:170] 0 pods were kept as unschedulable based on caching
I0208 13:02:42.010118       1 filter_out_schedulable.go:171] 5 pods marked as unschedulable can be scheduled.
I0208 13:02:42.010127       1 filter_out_schedulable.go:79] Schedulable pods present
I0208 13:02:42.010150       1 static_autoscaler.go:401] No unschedulable pods
I0208 13:02:42.010165       1 static_autoscaler.go:448] Calculating unneeded nodes
I0208 13:02:42.010190       1 pre_filtering_processor.go:57] Skipping ip-10-100-2-230.us-east-2.compute.internal - no node group config
I0208 13:02:42.010218       1 scale_down.go:443] Node ip-10-100-3-92.us-east-2.compute.internal is not suitable for removal - cpu utilization too big (0.945596)
I0208 13:02:42.010243       1 scale_down.go:447] Node ip-10-100-1-233.us-east-2.compute.internal - cpu utilization 0.064767
I0208 13:02:42.010264       1 scale_down.go:447] Node ip-10-100-2-108.us-east-2.compute.internal - cpu utilization 0.064767
I0208 13:02:42.010284       1 scale_down.go:443] Node ip-10-100-1-149.us-east-2.compute.internal is not suitable for removal - cpu utilization too big (0.893782)
I0208 13:02:42.010349       1 static_autoscaler.go:491] ip-10-100-1-233.us-east-2.compute.internal is unneeded since 2023-02-08 13:02:31.989144946 +0000 UTC m=+93.192107693 duration 10.020114811s
I0208 13:02:42.010366       1 static_autoscaler.go:491] ip-10-100-2-108.us-east-2.compute.internal is unneeded since 2023-02-08 13:02:31.989144946 +0000 UTC m=+93.192107693 duration 10.020114811s
I0208 13:02:42.010400       1 static_autoscaler.go:502] Scale down status: unneededOnly=true lastScaleUpTime=2023-02-08 13:02:21.799658894 +0000 UTC m=+83.002621633 lastScaleDownDeleteTime=2023-02-08 13:01:51.401419765 +0000 UTC m=+52.604382495 lastScaleDownFailTime=2023-02-08 13:01:51.401419845 +0000 UTC m=+52.604382575 scaleDownForbidden=true isDeleteInProgress=false scaleDownInCooldown=true
I0208 13:02:52.028167       1 static_autoscaler.go:228] Starting main loop
I0208 13:02:52.100706       1 auto_scaling_groups.go:351] Regenerating instance to ASG map for ASGs: [eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f]
I0208 13:02:52.153615       1 auto_scaling.go:199] 1 launch configurations already in cache
I0208 13:02:52.153847       1 aws_manager.go:269] Refreshed ASG list, next refresh after 2023-02-08 13:03:52.153840425 +0000 UTC m=+173.356803173
I0208 13:02:52.154884       1 filter_out_schedulable.go:65] Filtering out schedulables
I0208 13:02:52.155015       1 filter_out_schedulable.go:132] Filtered out 0 pods using hints
I0208 13:02:52.155167       1 filter_out_schedulable.go:157] Pod udacity.bloaty-mcbloatface-58c98b98d8-jcld5 marked as unschedulable can be scheduled on node template-node-for-eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f-6789034556239763083-0. Ignoring in scale up.
I0208 13:02:52.155246       1 filter_out_schedulable.go:157] Pod udacity.bloaty-mcbloatface-58c98b98d8-8kzdk marked as unschedulable can be scheduled on node template-node-for-eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f-6789034556239763083-0. Ignoring in scale up.
I0208 13:02:52.155318       1 filter_out_schedulable.go:157] Pod udacity.bloaty-mcbloatface-58c98b98d8-7sbjl marked as unschedulable can be scheduled on node template-node-for-eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f-6789034556239763083-0. Ignoring in scale up.
I0208 13:02:52.155386       1 filter_out_schedulable.go:157] Pod udacity.bloaty-mcbloatface-58c98b98d8-4fqb8 marked as unschedulable can be scheduled on node template-node-for-eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f-6789034556239763083-0. Ignoring in scale up.
I0208 13:02:52.155456       1 filter_out_schedulable.go:157] Pod udacity.bloaty-mcbloatface-58c98b98d8-tfvbz marked as unschedulable can be scheduled on node template-node-for-eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f-6789034556239763083-0. Ignoring in scale up.
I0208 13:02:52.155498       1 filter_out_schedulable.go:170] 0 pods were kept as unschedulable based on caching
I0208 13:02:52.155528       1 filter_out_schedulable.go:171] 5 pods marked as unschedulable can be scheduled.
I0208 13:02:52.155575       1 filter_out_schedulable.go:79] Schedulable pods present
I0208 13:02:52.155632       1 static_autoscaler.go:401] No unschedulable pods
I0208 13:02:52.155691       1 static_autoscaler.go:448] Calculating unneeded nodes
I0208 13:02:52.155800       1 scale_down.go:443] Node ip-10-100-3-92.us-east-2.compute.internal is not suitable for removal - cpu utilization too big (0.945596)
I0208 13:02:52.155863       1 scale_down.go:447] Node ip-10-100-1-233.us-east-2.compute.internal - cpu utilization 0.064767
I0208 13:02:52.155945       1 scale_down.go:447] Node ip-10-100-2-108.us-east-2.compute.internal - cpu utilization 0.064767
I0208 13:02:52.156008       1 scale_down.go:443] Node ip-10-100-1-149.us-east-2.compute.internal is not suitable for removal - cpu utilization too big (0.893782)
I0208 13:02:52.156066       1 scale_down.go:447] Node ip-10-100-2-230.us-east-2.compute.internal - cpu utilization 0.064767
I0208 13:02:52.156118       1 scale_down.go:447] Node ip-10-100-2-227.us-east-2.compute.internal - cpu utilization 0.064767
I0208 13:02:52.156174       1 scale_down.go:447] Node ip-10-100-1-216.us-east-2.compute.internal - cpu utilization 0.064767
I0208 13:02:52.156289       1 scale_down.go:564] Finding additional 1 candidates for scale down.
I0208 13:02:52.156325       1 cluster.go:148] Fast evaluation: ip-10-100-1-216.us-east-2.compute.internal for removal
I0208 13:02:52.156353       1 cluster.go:187] Fast evaluation: node ip-10-100-1-216.us-east-2.compute.internal may be removed
I0208 13:02:52.156437       1 static_autoscaler.go:491] ip-10-100-2-230.us-east-2.compute.internal is unneeded since 2023-02-08 13:02:52.02813276 +0000 UTC m=+113.231095502 duration 0s
I0208 13:02:52.156488       1 static_autoscaler.go:491] ip-10-100-2-227.us-east-2.compute.internal is unneeded since 2023-02-08 13:02:52.02813276 +0000 UTC m=+113.231095502 duration 0s
I0208 13:02:52.156520       1 static_autoscaler.go:491] ip-10-100-1-216.us-east-2.compute.internal is unneeded since 2023-02-08 13:02:52.02813276 +0000 UTC m=+113.231095502 duration 0s
I0208 13:02:52.156566       1 static_autoscaler.go:491] ip-10-100-1-233.us-east-2.compute.internal is unneeded since 2023-02-08 13:02:31.989144946 +0000 UTC m=+93.192107693 duration 20.038987809s
I0208 13:02:52.156610       1 static_autoscaler.go:491] ip-10-100-2-108.us-east-2.compute.internal is unneeded since 2023-02-08 13:02:31.989144946 +0000 UTC m=+93.192107693 duration 20.038987809s
I0208 13:02:52.156647       1 static_autoscaler.go:502] Scale down status: unneededOnly=true lastScaleUpTime=2023-02-08 13:02:21.799658894 +0000 UTC m=+83.002621633 lastScaleDownDeleteTime=2023-02-08 13:01:51.401419765 +0000 UTC m=+52.604382495 lastScaleDownFailTime=2023-02-08 13:01:51.401419845 +0000 UTC m=+52.604382575 scaleDownForbidden=true isDeleteInProgress=false scaleDownInCooldown=true
I0208 13:03:02.172419       1 static_autoscaler.go:228] Starting main loop
I0208 13:03:02.173021       1 clusterstate.go:248] Scale up in group eks-app-udacity-node-group-9cc3184c-5e88-c5f2-a1ab-acb5a5835c4f finished successfully in 40.198212968s
I0208 13:03:02.173072       1 filter_out_schedulable.go:65] Filtering out schedulables
I0208 13:03:02.173081       1 filter_out_schedulable.go:132] Filtered out 0 pods using hints
I0208 13:03:02.173090       1 filter_out_schedulable.go:170] 0 pods were kept as unschedulable based on caching
I0208 13:03:02.173096       1 filter_out_schedulable.go:171] 0 pods marked as unschedulable can be scheduled.
I0208 13:03:02.173106       1 filter_out_schedulable.go:82] No schedulable pods
I0208 13:03:02.173132       1 static_autoscaler.go:401] No unschedulable pods
I0208 13:03:02.173150       1 static_autoscaler.go:448] Calculating unneeded nodes
I0208 13:03:02.173203       1 scale_down.go:443] Node ip-10-100-3-92.us-east-2.compute.internal is not suitable for removal - cpu utilization too big (0.945596)
I0208 13:03:02.173233       1 scale_down.go:447] Node ip-10-100-1-233.us-east-2.compute.internal - cpu utilization 0.064767
I0208 13:03:02.173251       1 scale_down.go:447] Node ip-10-100-2-108.us-east-2.compute.internal - cpu utilization 0.064767
I0208 13:03:02.173274       1 scale_down.go:443] Node ip-10-100-1-149.us-east-2.compute.internal is not suitable for removal - cpu utilization too big (0.893782)
I0208 13:03:02.173293       1 scale_down.go:443] Node ip-10-100-2-230.us-east-2.compute.internal is not suitable for removal - cpu utilization too big (0.712435)
I0208 13:03:02.173306       1 scale_down.go:447] Node ip-10-100-2-227.us-east-2.compute.internal - cpu utilization 0.064767
I0208 13:03:02.173318       1 scale_down.go:447] Node ip-10-100-1-216.us-east-2.compute.internal - cpu utilization 0.064767
I0208 13:03:02.173405       1 static_autoscaler.go:491] ip-10-100-1-233.us-east-2.compute.internal is unneeded since 2023-02-08 13:02:31.989144946 +0000 UTC m=+93.192107693 duration 30.183241859s
I0208 13:03:02.173420       1 static_autoscaler.go:491] ip-10-100-2-108.us-east-2.compute.internal is unneeded since 2023-02-08 13:02:31.989144946 +0000 UTC m=+93.192107693 duration 30.183241859s
I0208 13:03:02.173435       1 static_autoscaler.go:491] ip-10-100-2-227.us-east-2.compute.internal is unneeded since 2023-02-08 13:02:52.02813276 +0000 UTC m=+113.231095502 duration 10.14425405s
I0208 13:03:02.173451       1 static_autoscaler.go:491] ip-10-100-1-216.us-east-2.compute.internal is unneeded since 2023-02-08 13:02:52.02813276 +0000 UTC m=+113.231095502 duration 10.14425405s
I0208 13:03:02.173476       1 static_autoscaler.go:502] Scale down status: unneededOnly=true lastScaleUpTime=2023-02-08 13:02:21.799658894 +0000 UTC m=+83.002621633 lastScaleDownDeleteTime=2023-02-08 13:01:51.401419765 +0000 UTC m=+52.604382495 lastScaleDownFailTime=2023-02-08 13:01:51.401419845 +0000 UTC m=+52.604382575 scaleDownForbidden=false isDeleteInProgress=false scaleDownInCooldown=true
```

## Status of kubernetes cluster

```bash
Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl get deployments
NAME                 READY   UP-TO-DATE   AVAILABLE   AGE
bloaty-mcbloatface   17/17   17           17          10m
blue                 3/3     3            3           32m
canary-v1            3/3     3            3           33m
green                3/3     3            3           32m
hello-world          1/1     1            1           33m

Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl get nodes
NAME                                         STATUS   ROLES    AGE    VERSION
ip-10-100-1-233.us-east-2.compute.internal   Ready    <none>   115m   v1.21.14-eks-49d8fe8
ip-10-100-2-108.us-east-2.compute.internal   Ready    <none>   118s   v1.21.14-eks-49d8fe8
ip-10-100-3-81.us-east-2.compute.internal    Ready    <none>   116m   v1.21.14-eks-49d8fe8

Stephane@Surface-de-Stef MINGW64 ~/OneDrive/Documents/Udacity SRE/Project 3/nd087-c3-deployment-roulette (main)
$ kubectl get pods
NAME                                  READY   STATUS    RESTARTS   AGE
bloaty-mcbloatface-58c98b98d8-22tc4   1/1     Running   0          71s
bloaty-mcbloatface-58c98b98d8-5cr4g   1/1     Running   0          71s
bloaty-mcbloatface-58c98b98d8-79pmq   1/1     Running   0          71s
bloaty-mcbloatface-58c98b98d8-9gfwv   1/1     Running   0          70s
bloaty-mcbloatface-58c98b98d8-d42qq   1/1     Running   0          71s
bloaty-mcbloatface-58c98b98d8-h7m9s   1/1     Running   0          71s
bloaty-mcbloatface-58c98b98d8-jt9vz   1/1     Running   0          71s
bloaty-mcbloatface-58c98b98d8-ld8lh   1/1     Running   0          71s
bloaty-mcbloatface-58c98b98d8-lxm5f   1/1     Running   0          71s
bloaty-mcbloatface-58c98b98d8-mn59n   1/1     Running   0          70s
bloaty-mcbloatface-58c98b98d8-mz5bh   1/1     Running   0          71s
bloaty-mcbloatface-58c98b98d8-prfrt   1/1     Running   0          71s
bloaty-mcbloatface-58c98b98d8-q24hs   1/1     Running   0          71s
bloaty-mcbloatface-58c98b98d8-rv6tz   1/1     Running   0          70s
bloaty-mcbloatface-58c98b98d8-smbtb   1/1     Running   0          70s
bloaty-mcbloatface-58c98b98d8-v679g   1/1     Running   0          71s
bloaty-mcbloatface-58c98b98d8-vv7s9   1/1     Running   0          71s
blue-8475cbdf46-9wpg9                 1/1     Running   0          2m30s
blue-8475cbdf46-pb7rt                 1/1     Running   0          2m30s
blue-8475cbdf46-sl4q8                 1/1     Running   0          2m30s
canary-v1-64598c676f-55vdn            1/1     Running   0          2m33s
canary-v1-64598c676f-ltz6h            1/1     Running   0          2m33s
canary-v1-64598c676f-rv8mq            1/1     Running   0          2m33s
green-5cdd96c9b4-2ghld                1/1     Running   0          2m30s
green-5cdd96c9b4-d656v                1/1     Running   0          2m30s
green-5cdd96c9b4-tg4bh                1/1     Running   0          2m30s
hello-world-794458d64d-nxv6r          1/1     Running   0          2m39s
```