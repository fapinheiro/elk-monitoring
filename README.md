# elk-monitoring
A project for monitoring k8s cluster with ELK

# Requirements

```
kubectl version 
helm version 
```

Must be at least:

kubectl 1.16+
helm 3+

# ElasticSearch
Install as a replica set in at least 2 nodes

Elastic is configured as a Stateful a special type of ReplicaSet, k8s wont give a random name for each POD.

*** Warning: Get the default values and change the file elasticsearch\elasticsearch.yaml regarding the chart version. Get default values at  https://artifacthub.io/packages/helm/elastic/elasticsearch ***

```
helm repo add elastic https://helm.elastic.co
helm repo update
helm search repo elastic/elasticsearch
helm show values elastic/elasticsearch
helm install -n default -f elasticsearch/elasticsearch.yaml elasticsearch elastic/elasticsearch --version=7.17.1
```


# Fluentd 
Install FluentD in the namespaces you need to collect data

FluentD is configured as a DaemonSet is like a ReplicaSet, but this tells k8s to run on every single node. A special type of workload.

Lista de Datasources https://www.fluentd.org/datasources

Kubernetes official
https://github.com/kubernetes/kubernetes/tree/master/cluster/addons/fluentd-elasticsearch


*** Warning: Get the default values and change the file fluentd\fluentd.yaml regarding the chart version. Get default values at  https://artifacthub.io/packages/helm/bitnami/fluentd ***

*** Warning: After changing default values, do not forget to change standard output to elasticsearch ***


```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm search repo bitnami/fluentd
helm show values bitnami/fluentd
helm install -n default -f fluentd/fluentd.yaml fluentd bitnami/fluentd --version=5.0.0
```


# Kibana
Install as a replica set in at least 1 node

No command is needed here

*** Warning: Get the default values and change the file kibana\kibana.yaml regarding the chart version. Get default values at  https://artifacthub.io/packages/helm/elastic/elasticsearch ***

*** Warning: After changing default values, do not forget to change elasticsearch host ***

```
helm repo add elastic https://helm.elastic.co
helm repo update
helm search repo elastic/kibana
helm show values elastic/kibana
helm install -n default -f kibana\kibana.yaml kibana elastic/kibana --version=7.17.1
```

Optional

```
kubectl --namespace default port-forward  $(kubectl get pods --namespace default -l "app=kibana" -o jsonpath="{.items[0].metadata.name}") 5601:5601
```
 
## Setup index
Go to Kibana management in order to create a new index

- Create index pattern "logstash*"
- Create index suffix "@timestamp"

## Discover
If everything is ok, go to Kibana Discover and have fun :)


# References
https://artifacthub.io/packages/helm/bitnami/fluentd
https://artifacthub.io/packages/helm/elastic/elasticsearch
https://artifacthub.io/packages/helm/elastic/kibana