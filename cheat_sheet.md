# Kubectl commands


lists all available contexts
```sh
kubectl config get-contexts
```
switches your active Kubernetes context
```sh
kubectl config use-context {cluster}
```
sets a default namespace
```sh
kubectl config set-context $(kubectl config current context) —namespace={namespace}
```
creates a single Pod
```sh
kubectl run nginx —image={image}
```
 create’s a kubernetes resource from a yaml file
 ```sh
 kubectl create -f {filename.yaml}
 ```
applies the configuration defined in the definition.yaml or creates the resource (if not exists)
```sh
kubectl apply -f {definition.yaml}
```
Forcibly replaces a Kubernetes resource
```sh
kubectl replace —force -f {definition.yaml}
```
simulates creating a Pod and outputs the yaml definition
```sh
kubectl run nginx —image={image} --dry-run=client -o yaml
```
simulates creating a Pod  and creates a file with the yaml definition
```sh
kubectl run redis —image={image} --dry-run=client -o yaml > {filename}.yaml
``` 
create deployment
```sh       
kubectl create deployment —image={image}
```
creates a deployment, 4 replicas, just outputs it, formats the output as yaml
```sh
kubectl create deployment —image={image} --dry-run=client -o yaml
```
 creates a deployment, 4 replicas, just outputs it, formats the output as YAML, saves the yaml
 ```sh
 kubectl create deployment —-image={image} --replicas=4 --dry-run=client -o yaml > {filename}.yaml
 ```
 delete pod, replicaset, service, deployment
 ```sh
 kubectl delete pod, replicaset, service, deployment
 ```
show pod, deployment, service, replicaset
```sh
 kubectl get pods, deployment, service, replicaset
```
get all pods from all namespaces
```sh
kubectl get pods —all-namespaces
```
list all the main Kubernetes resources
```sh
kubectl get all
```
create object
```sh
kubectl create -f {filename.yaml}
```
updates the number of replicas (temporary, yaml definition is not update)
```sh
kubectl scale —replicas={amount} -f {filename.yaml}
```
opens the specified ReplicaSet resource in your default text editor (edit is temporary)
```sh
Kubectl edit replicaset {name}
```
run pod and declares port
```sh
kubectl run {podname} --image={image} --port=8080
```
displays a list of events
```sh
kubectl get events
```
kubectl apply -f {definition.yaml}


