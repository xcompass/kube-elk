ELK Cluster on Kubernetes
-------------------------
All the docker images are the latest version using official repo.

## Config

You will need to provide a NFS mount for *Elasticsearch* storage. Update nfs-pv.yml for your NFS IP. You can also use other storage solution that supported by *Kuberentes*.

## Deploy

```
kubectl create -f service-account.yaml
kubectl create -f nfs-pv.yml
kubectl create -f nfs-pvc.yml
kubectl create -f elasticsearch-svc.yml
kubectl create -f elasticsearch-rc.yml
kubectl create -f logstash-svc.yaml
kubectl create -f logstash-rc.yaml
kubectl create -f kibana-svc.yaml
kubectl create -f kibana-rc.yaml
```

To check if the pods are created:

```
$ kubectl get pods
NAME                  READY     STATUS    RESTARTS   AGE
elasticsearch-0rr4o   1/1       Running   0          10s
logstash-c9z1o        1/1       Running   0          13s
kibana-4qktn          1/1       Running   0          15s
```
It will create one container for each component. You can modify the controller file (-rc.yaml) to add more replicas. 

## Access

For *Kibana*, the webUI is on port 30045. So you can access from any node IP. If you have a load balancer, you can set it up to forward the request to 30045.
For *Logstash*, there are two ports exported on the node, 30043 and 30044. Port 30043 is for lamberjack and 30044 is pure tcp input. 
