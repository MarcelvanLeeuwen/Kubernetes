# Kubectl

kubectl config get-contexts --> available clusters

kubectl config use-context {cluster} --> switch to cluster

kubectl run nginx —image=nginx

kubectl run nginx —image=nginx --dry-run=client -o yaml --> outputs the yaml definition

kubectl run redis —image=redis --dry-run=client -o yaml --> redis.yaml. —> creates a file with the yaml definition

kubectl create deployment —image=nginx

kubectl create deployment —image=nginx --dry-run=client -o yaml

kubectl create deployment —image=nginx --replicas=4 --dry-run=client -o yaml > nginx.yaml

kubectl create -f {filename.yaml} --> create’s a kubernetes resource from a yaml file.

kubectl delete pod, replicaset, service, deployment 

kubectl get pods, deployment, service, replicaset

kubectl get all

kubectl create -f {filename.yaml} --> this can be services, replicasets, deployments. 

kubectl scale —replicas=6 -f {file.yaml} --> updates the number of replicas / is temporary because the yaml definition is not update

Kubectl edit replicaset {name} --> you can edit the amount of replicas this way too, but it is temporary. Edit yaml file if you want to make it persistent.