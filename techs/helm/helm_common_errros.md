# Error: configmaps is forbidden: User "system:serviceaccount:kube-system:tiller" cannot list resource "configmaps" in API group "" in the namespace "kube-system"

Example: 

jigishkp01-mac:kubernetes jigishkp$ helm ls -a
Error: configmaps is forbidden: User "system:serviceaccount:kube-system:tiller" cannot list resource "configmaps" in API group "" in the namespace "kube-system"

Solution : look https://github.com/helm/helm/issues/5100

jigishkp01-mac:kubernetes jigishkp$ kubectl --namespace kube-system get clusterrolebinding
NAME                                                   AGE
cluster-admin                                          23m
cluster-autoscaler-updateinfo                          22m
event-exporter-rb                                      22m
gce:beta:kubelet-certificate-bootstrap                 22m
gce:beta:kubelet-certificate-rotation                  22m
gce:cloud-provider                                     22m
heapster-binding                                       22m
kube-apiserver-kubelet-api-admin                       22m
kubelet-bootstrap                                      22m
kubelet-bootstrap-certificate-bootstrap                22m
kubelet-bootstrap-node-bootstrapper                    22m
kubelet-cluster-admin                                  22m
metrics-server:system:auth-delegator                   22m
npd-binding                                            22m
stackdriver:fluentd-gcp                                22m
stackdriver:metadata-agent                             22m
system:aws-cloud-provider                              23m
system:basic-user                                      23m
system:controller:attachdetach-controller              23m
system:controller:certificate-controller               23m
system:controller:clusterrole-aggregation-controller   23m
system:controller:cronjob-controller                   23m
system:controller:daemon-set-controller                23m
system:controller:deployment-controller                23m
system:controller:disruption-controller                23m
system:controller:endpoint-controller                  23m
system:controller:expand-controller                    23m
system:controller:generic-garbage-collector            23m
system:controller:horizontal-pod-autoscaler            23m
system:controller:job-controller                       23m
system:controller:namespace-controller                 23m
system:controller:node-controller                      23m
system:controller:persistent-volume-binder             23m
system:controller:pod-garbage-collector                23m
system:controller:pv-protection-controller             23m
system:controller:pvc-protection-controller            23m
system:controller:replicaset-controller                23m
system:controller:replication-controller               23m
system:controller:resourcequota-controller             23m
system:controller:route-controller                     23m
system:controller:service-account-controller           23m
system:controller:service-controller                   23m
system:controller:statefulset-controller               23m
system:controller:ttl-controller                       23m
system:discovery                                       23m
system:kube-controller-manager                         23m
system:kube-dns                                        23m
system:kube-dns-autoscaler                             22m
system:kube-scheduler                                  23m
system:metrics-server                                  22m
system:node                                            23m
system:node-proxier                                    23m
system:public-info-viewer                              23m
system:volume-scheduler                                23m
jigishkp01-mac:kubernetes jigishkp$ kubectl --namespace kube-system create clusterrolebinding tiller-cluster-admin --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
clusterrolebinding.rbac.authorization.k8s.io "tiller-cluster-admin" created

# Error: could not find tiller

jigishkp01-mac:kubernetes jigishkp$ helm version
Client: &version.Version{SemVer:"v2.16.6", GitCommit:"dd2e5695da88625b190e6b22e9542550ab503a47", GitTreeState:"clean"}
Error: could not find tiller

Solution: Run below commands. 

$ kubectl -n kube-system create serviceaccount tiller
$ helm init --service-account tiller
$ kubectl --namespace kube-system patch deploy tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'
$ kubectl --namespace kube-system get deploy tiller-deploy -o yaml

sample out put below 

jigishkp01-mac:kubernetes jigishkp$ kubectl -n kube-system create serviceaccount tiller
serviceaccount "tiller" created
jigishkp01-mac:kubernetes jigishkp$ helm init --service-account tiller
$HELM_HOME has been configured at /Users/jigishkp/.helm.

Tiller (the Helm server-side component) has been installed into your Kubernetes Cluster.

Please note: by default, Tiller is deployed with an insecure 'allow unauthenticated users' policy.
To prevent this, run `helm init` with the --tiller-tls-verify flag.
For more information on securing your installation see: https://v2.helm.sh/docs/securing_installation/
jigishkp01-mac:kubernetes jigishkp$ kubectl --namespace kube-system patch deploy tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'
deployment.extensions "tiller-deploy" not patched
jigishkp01-mac:kubernetes jigishkp$ kubectl --namespace kube-system get deploy tiller-deploy -o yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
  creationTimestamp: 2020-06-02T21:33:32Z
  generation: 1
  labels:
    app: helm
    name: tiller
  name: tiller-deploy
  namespace: kube-system
  resourceVersion: "1647"
  selfLink: /apis/extensions/v1beta1/namespaces/kube-system/deployments/tiller-deploy
  uid: b4e3221a-a518-11ea-bbf0-42010af0018d
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: helm
      name: tiller
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: helm
        name: tiller
    spec:
      automountServiceAccountToken: true
      containers:
      - env:
        - name: TILLER_NAMESPACE
          value: kube-system
        - name: TILLER_HISTORY_MAX
          value: "0"
        image: gcr.io/kubernetes-helm/tiller:v2.16.6
        imagePullPolicy: IfNotPresent
        livenessProbe:
          failureThreshold: 3
          httpGet:
            path: /liveness
            port: 44135
            scheme: HTTP
          initialDelaySeconds: 1
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 1
        name: tiller
        ports:
        - containerPort: 44134
          name: tiller
          protocol: TCP
        - containerPort: 44135
          name: http
          protocol: TCP
        readinessProbe:
          failureThreshold: 3
          httpGet:
            path: /readiness
            port: 44135
            scheme: HTTP
          initialDelaySeconds: 1
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 1
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      serviceAccount: tiller
      serviceAccountName: tiller
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 1
  conditions:
  - lastTransitionTime: 2020-06-02T21:33:45Z
    lastUpdateTime: 2020-06-02T21:33:45Z
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: 2020-06-02T21:33:32Z
    lastUpdateTime: 2020-06-02T21:33:45Z
    message: ReplicaSet "tiller-deploy-8756df4d9" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  observedGeneration: 1
  readyReplicas: 1
  replicas: 1
  updatedReplicas: 1