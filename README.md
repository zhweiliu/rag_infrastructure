# rag_infrastructure
Managed k8s resources by argocd

k3d node create commands : 
```shell
$ k3d node create postgresql --role agent --cluster infrastructure --k3s-node-label "cloud.google.com/gke-nodepool=postgresql"
```