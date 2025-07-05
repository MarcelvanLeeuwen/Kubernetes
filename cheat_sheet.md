# Kubectl commands


- Lists all available contexts
```sh
kubectl config get-contexts
```
- Switches your active Kubernetes context
```sh
kubectl config use-context {cluster}
```
- Sets a default namespace
```sh
kubectl config set-context $(kubectl config current context) —namespace={namespace}
```
- Creates a single Pod
```sh
kubectl run nginx —image={image}
```
 - Creates a kubernetes resource from a yaml file
 ```sh
 kubectl create -f {filename.yaml}
 ```
- Applies the configuration defined in the definition.yaml or creates the resource (if not exists)
```sh
kubectl apply -f {definition.yaml}
```
- Forcibly replaces a Kubernetes resource
```sh
kubectl replace —force -f {definition.yaml}
```
- Simulates creating a Pod and outputs the yaml definition
```sh
kubectl run nginx —image={image} --dry-run=client -o yaml
```
- Simulates creating a Pod  and creates a file with the yaml definition
```sh
kubectl run redis —image={image} --dry-run=client -o yaml > {filename}.yaml
``` 
- Create deployment
```sh       
kubectl create deployment —image={image}
```
- Creates a deployment, 4 replicas, just outputs it, formats the output as yaml
```sh
kubectl create deployment —image={image} --dry-run=client -o yaml
```
 - Creates a deployment, 4 replicas, just outputs it, formats the output as YAML, saves the yaml
 ```sh
 kubectl create deployment —-image={image} --replicas=4 --dry-run=client -o yaml > {filename}.yaml
 ```
 - Delete pod, replicaset, service, deployment
 ```sh
 kubectl delete pod, replicaset, service, deployment
 ```
- Show pod, deployment, service, replicaset
```sh
 kubectl get pods, deployment, service, replicaset
```
- Get all pods from all namespaces
```sh
kubectl get pods —all-namespaces
```
- List all the main Kubernetes resources
```sh
kubectl get all
```
- Create object
```sh
kubectl create -f {filename.yaml}
```
- Updates the number of replicas (temporary, yaml definition is not update)
```sh
kubectl scale —replicas={amount} -f {filename.yaml}
```
- Opens the specified ReplicaSet resource in your default text editor (edit is temporary)
```sh
Kubectl edit replicaset {name}
```
- Run pod and declares port
```sh
kubectl run {podname} --image={image} --port=8080
```
- Displays a list of events
```sh
kubectl get events
```



