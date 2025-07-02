# Kubectl


available clusters

```sh
kubectl config get-contexts
```
switch to cluster
```sh
kubectl config use-context {cluster}
```
creates a single Pod
```sh
kubectl run nginx —image=nginx
```
outputs the yaml definition
```sh
kubectl run nginx —image=nginx --dry-run=client -o yaml
```
creates a file with the yaml definition
```sh
kubectl run redis —image=redis --dry-run=client -o yaml > redis.yaml
``` 
create deployment
```sh       
kubectl create deployment —image=nginx
```
creates a deployment, 4 replicas, just outputs it, formats the output as yaml
```sh
kubectl create deployment —image=nginx --dry-run=client -o yaml
```
 creates a deployment, 4 replicas, just outputs it, formats the output as YAML, saves the yaml
 ```sh
 kubectl create deployment —-image=nginx --replicas=4 --dry-run=client -o yaml > nginx.yaml
 ```
 create’s a kubernetes resource from a yaml file
 ```sh
 kubectl create -f {filename.yaml}
 ```
 delete pod, replicaset, service, deployment
 ```sh
 kubectl delete pod, replicaset, service, deployment
 ```
show pod, deployment, service, replicaset
```sh
 kubectl get pods, deployment, service, replicaset
```
list all the main Kubernetes resources
```sh
kubectl get all
```
create object
```sh
kubectl create -f {filename.yaml}
```
updates the number of replicas (temporary) yaml definition is not update
```sh
kubectl scale —replicas=6 -f {file.yaml}
```
you can edit the amount of replicas this way too (temporary)
```sh
Kubectl edit replicaset {name}
```