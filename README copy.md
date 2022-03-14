# elk-monitoring
A project for monitoring k8s cluester with ELK

# Volume
Create a volume to be used for Elastic search

```
kubectl apply -f <provider>-elastic-storage.yml
```

# FluentD 
Install FluentD/Logstash in every single node on the k8s cluster so that it can find containers logs

FluentD is configured as a DaemonSet is like a ReplicaSet, but this tells k8s to run on every single node. A special type of workload.

Lista de Datasources https://www.fluentd.org/datasources

Kubernetes official
https://github.com/kubernetes/kubernetes/tree/master/cluster/addons/fluentd-elasticsearch

All those files are combined in two:
- elastic-stack.yaml
- fluentd-config.yaml

```
kubectl apply -f fluentd-config.yml
```

# ElasticSearch
Install as a replica set in at least 2 nodes

Elastic is configured as a Stateful a special type of ReplicaSet, k8s wont give a random name for each POD.

*** Warning: In the elastic-stack, the name of elk-disk must match the provider disk claim name ***

```
kubectl apply -f elastic-stack.yml
```

# Kibana
Install as a replica set in at least 1 node

No command is needed here

## Setup index
Go to Kibana management in order to create a new index

- Create index pattern "logstash*"
- Create index suffix "@timestamp"

## Discover
If everything is ok, go to Kibana Discover and have fun :)

# Useful commands

## Get pods
Check if all pods are running

```
kubectl get po -n kube-system
```


## Get pods logs
Get logs for a specifc POD

```
kubectl logs <nameofthepode>
```


## Get services
Check if you have Kibana and Elastic services

```
kubectl get svc -n kube-system
```

## Get Kibana endpoint
```
kubectl describe svc kibana-logging -n kube-system
```

# Annotations



