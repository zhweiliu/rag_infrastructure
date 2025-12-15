# rag_infrastructure
Managed k8s resources by argocd

(manual apply by once) apply applicationset command: 
```shell
kubectl apply -f argocd\clusters\apps\applicationset.yaml
```

k3d node create commands : 
```shell
$ k3d node create postgresql --role agent --cluster infrastructure --k3s-node-label "cloud.google.com/gke-nodepool=postgresql"
```