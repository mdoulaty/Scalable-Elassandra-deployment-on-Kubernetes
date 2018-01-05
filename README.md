# Scalable multi-node Elassandra deployment on Kubernetes Cluster
This recipe is mostly based on [this](https://github.com/IBM/scalable-cassandra-deployment-on-kubernetes) repository, but it is modified for [Elassandra](https://github.com/strapdata/elassandra).
It uses statefulsets for storing data persistantly and it is easy to scale up and down!

It is assumed that you are already familiary with Cassandra, EalsticSearch and Kubernetes and you have a working Kubernetes cluster. 

## tldr;
```
$ kubectl create -f elassandra-service.yaml
$ kubectl create -f local-volumes.yaml
$ kubectl create -f elassandra-statefulset.yaml
```

## Elassandra service
First the Elassandra service is defined and the relevant ports are exposed:

`$ kubectl -f elassandra-service.yaml`

Check to verify the service is created:

`$ kubectl get svc elassandra`


## Local volumes
Then the local volumes are created:

`$ kubectl create -f local-volumes.yaml`

To verify it:

`$ kubectl get pv`

## Statefulsets
Then the pods are created using the statefulset definition:

`$ kubectl create -f elassandra-statefulset.yaml`

And to verify the statefulset:

`$ kubectl get statefulsets`

## Scaling up and down
With this sample script, only three persistant volumes are created - so if you want to scale to more than three pods, first create the pvs accordingly.

`$ kubectl scale statefulset elassandra --replicas=3`

And to verify the state of the statefulset:

`$ kubectl get statefulsets`

## Accessing the cluster
Assuming that the default namespace was used, the service is accessible via `elassandra.svc.default.cluster.local`

## Checking Cassandra nodes:
`$ kubectl exec -ti elassandra-0 nodetool status`

## Checking ElasticSearch status:
`$ curl http://elassandra.svc.default.cluster.local:9200`
