# operator-meetup

This repository contains all the material that was used to present the Essential Data Meetup of March 6, 2019 on the Operators.

# slides

[Operators on Google slides](https://docs.google.com/presentation/d/1hJZFsOnbILe26SJre5RXLTbqug8XHnEs3Otn5hqeBuw/edit?usp=sharing)

# Demo

create manage k8S sur gcloud (project postgres) :

--- etcd operator ---

install olm :

$ kubectl create -f https://raw.githubusercontent.com/operator-framework/operator-lifecycle-manager/master/deploy/upstream/quickstart/olm.yaml

check deployment :

$ kubectl get po --all-namespaces -w

install etcd operator via olm :

$ kubectl create -f https://www.operatorhub.io/install/etcdoperator-community.v0.9.2.yaml

check deployment :

$ kubectl get po --all-namespaces -w

create cluster with version 3.1.16 :

$ kubectl apply -f etcd-cluster-oldver.yml

check deployment :

$ kubectl get po -n my-etcd -w

forward etcd service on local:

$ kubectl --namespace my-etcd port-forward service/etcd-cluster-client 2379:2379     

be sure to be on the v3 api of etcd for etcdctl:

$ export ETCDCTL_API=3

check version of deployed cluster :

$ ./etcdctl endpoint status

add some data :

$ ./etcdctl --endpoints http://127.0.0.1:2379 put foo bar

check if data is there : 

$ ./etcdctl --endpoints http://127.0.0.1:2379 get foo

destroy a pod : 

$ kubectl delete -n my-etcd po/

restart forward :

$ kubectl --namespace my-etcd port-forward service/etcd-cluster-client 2379:2379   

check if data is there : 

$ ./etcdctl --endpoints http://127.0.0.1:2379 get foo

update version of etcd :

$ kubectl apply -f etcd-cluster.yml

check update :

$ kubectl get po -n my-etcd -w

restart forward if necessary :

$ kubectl --namespace my-etcd port-forward service/etcd-cluster-client 2379:2379

check version of deployed cluster :

$ ./etcdctl endpoint status

check if data is still there : 

$ ./etcdctl --endpoints http://127.0.0.1:2379 get foo

in order to access s3 add secret :

$ kubectl -n my-etcd create secret generic aws --from-file=credentials --from-file=config

backup the data :

$ kubectl apply -f etcd-backup.yml

check on aws :  
...

delete data on the cluster :

$ ./etcdctl --endpoints http://127.0.0.1:2379 del foo

check if data doesn't exist anymore on the cluster :

$ ./etcdctl --endpoints http://127.0.0.1:2379 get foo

restore data :

$ kubectl apply -f etcd-restore.yml

check if data as been restore on the cluster :

$ ./etcdctl --endpoints http://127.0.0.1:2379 get foo