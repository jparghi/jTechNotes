kubernetes
==========

#. creating kubectl alias** 

alias k=kubectl

#. CHECK RUNNING PODS
$ kubectl get pods -n onap
NAME                           READY     STATUS    RESTARTS   AGE
frank-cassandra-0              0/1       Pending   0          25s
frank-robot-54bbcb5987-qhmct   0/1       Running   0          3m



$ kubectl version
Client Version: version.Info{Major:"1", Minor:"10", GitVersion:"v1.10.0", GitCommit:"fc32d2f3698e36b93322a3465f63a14e9f0eaead", GitTreeState:"clean", BuildDate:"2018-03-26T16:55:54Z", GoVersion:"go1.9.3", Compiler:"gc", Platform:"darwin/amd64"}
Server Version: version.Info{Major:"1", Minor:"14+", GitVersion:"v1.14.10-gke.36", GitCommit:"34a615f32e9a0c9e97cdb9f749adb392758349a6", GitTreeState:"clean", BuildDate:"2020-04-06T16:33:17Z", GoVersion:"go1.12.12b4", Compiler:"gc", Platform:"linux/amd64"}

**Access the POD**

kubectl exec -it onap-jparghi-so-5997548d8-8kk7j bash

Getting the namespace 

$ kubectl get namespace

Getting all the services with details. 

$ kubectl get service -o wide

Getting pods with ip 

$ kubectl get pods -o wide

Copy log files using kubectl 

$ kubectl cp fairy-sdnc-sdnc-0:/var/log/onap/sdnc/karaf.log Downloads

Edit SDNC pod for changing configuration 

$ kubectl edit svc sdnc

Kubectl describe pod

$ kubectl describe pods onap-jparghi-so-6c95d8fb9b-vswsr

Get IPs for the environment

$ kubectl get nodes -o wide

Delete PODS

kubectl delete pods <pod> 
kubectl delete pods <pod> --grace-period=0 --force

Edit Deployment for any POD

kubectl get deployments
kubectl edit deployment onap-dmaap-message-router-kafka
kubectl edit deployment onap-ves-transformer

Play with Configmap 

kubectl get configmaps
kubectl get configmaps ves-configmap -o=yaml
kubectl delete configmap onap-ves-transformer.v1

Going to Persisted Volume on K8s Cluster

kubectl -n default exec -it ltec-dev-nfs-nfs-client-provisioner-87b6c85f4-rxtpd sh
/persistentvolumes/royal/onap/casablanca/casaint-onap-dmaap/message-router/data-kafka/logs

Deleting Persistent volume 

https://kubernetes.io/docs/tasks/administer-cluster/change-pv-reclaim-policy/


# Describe commands with verbose output
kubectl describe nodes my-node
kubectl describe pods my-pod

# Interacting with running Pods

kubectl logs my-pod                                 # dump pod logs (stdout)
kubectl logs -l name=myLabel                        # dump pod logs, with label name=myLabel (stdout)
kubectl logs my-pod --previous                      # dump pod logs (stdout) for a previous instantiation of a container
kubectl logs my-pod -c my-container                 # dump pod container logs (stdout, multi-container case)
kubectl logs -l name=myLabel -c my-container        # dump pod logs, with label name=myLabel (stdout)
kubectl logs my-pod -c my-container --previous      # dump pod container logs (stdout, multi-container case) for a previous instantiation of a container
kubectl logs -f my-pod                              # stream pod logs (stdout)
kubectl logs -f my-pod -c my-container              # stream pod container logs (stdout, multi-container case)
kubectl logs -f -l name=myLabel --all-containers    # stream all pods logs with label name=myLabel (stdout)
kubectl run -i --tty busybox --image=busybox -- sh  # Run pod as interactive shell
kubectl run nginx --image=nginx --restart=Never -n 
mynamespace                                         # Run pod nginx in a specific namespace
kubectl run nginx --image=nginx --restart=Never     # Run pod nginx and write its spec into a file called pod.yaml
--dry-run -o yaml > pod.yaml

kubectl attach my-pod -i                            # Attach to Running Container
kubectl port-forward my-pod 5000:6000               # Listen on port 5000 on the local machine and forward to port 6000 on my-pod
kubectl exec my-pod -- ls /                         # Run command in existing pod (1 container case)
kubectl exec my-pod -c my-container -- ls /         # Run command in existing pod (multi-container case)
kubectl top pod POD_NAME --containers               # Show metrics for a given pod and its containers#

# get nodes
$ kubectl get nodes
NAME                                  STATUS    ROLES     AGE       VERSION
gke-gke2-default-pool-c4ff49be-7fjr   Ready     <none>    4d        v1.14.10-gke.36
gke-gke2-default-pool-c4ff49be-gfmf   Ready     <none>    4d        v1.14.10-gke.36

# get services
$ kubectl get services
NAME         TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.15.240.1   <none>        443/TCP   4d

# delete namespace 
kubectl delete namespace onap

# delete pod(s)
kubectl delete pods -n onap --all

# delete PV
kubectl delete persistentvolumes -n onap --all

# get pvc 
$ kubectl get pvc -n onap

# get pv
$ kubectl get pv -n onap
