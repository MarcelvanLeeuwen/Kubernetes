# Kubectl

<table>
    <tr>
        <th style="width: 700px;">Command</th>
        <th style="width: 500px;">Explanation</th>
    <tr>
        <td>kubectl config get-contexts</td>
        <td>available clusters</td>
    </tr>
    <tr>
        <td>kubectl config use-context {cluster}</td>
        <td>switch to cluster</td>
    </tr>
    <tr>   
        <td>kubectl run nginx —image=nginx</td>
        <td>creates a single Pod</td>
    </tr>
    <tr>
        <td>kubectl run nginx —image=nginx --dry-run=client -o yaml</td>
        <td>outputs the yaml definition</td>
    </tr>
    <tr>
        <td>kubectl run redis —image=redis --dry-run=client -o yaml > redis.yaml</td>
        <td>creates a file with the yaml definition</td>
    </tr>
    <tr>
        <td>kubectl create deployment —image=nginx</td>
        <td>create deployment</td>
    </tr>
    <tr>    
        <td>kubectl create deployment —image=nginx --dry-run=client -o yaml</td>
        <td>creates a deployment, 4 replicas, just outputs it, formats the output as yaml</td>
    </tr>
    <tr>
        <td>kubectl create deployment —-image=nginx --replicas=4 --dry-run=client -o yaml > nginx.yaml</td>
        <td>creates a deployment, 4 replicas, just outputs it, formats the output as YAML, saves the YAML</td>
    </tr>
    <tr>
        <td>kubectl create -f {filename.yaml}</td>
        <td>create’s a kubernetes resource from a yaml file</td>
    </tr>
    <tr>
        <td>kubectl delete pod, replicaset, service, deployment</td>
        <td>delete pod, replicaset, service, deployment</td> 
    </tr>
    <tr>
        <td>kubectl get pods, deployment, service, replicaset</td>
        <td>show pod, deployment, service, replicaset</td>
    </tr>
    <tr>
        <td>kubectl get all</td>
        <td>list all the main Kubernetes resources</td>
    </tr>
    <tr>
        <td>kubectl create -f {filename.yaml}</td>
        <td>create object</td>
    </tr>
    <tr>
        <td>kubectl scale —replicas=6 -f {file.yaml}</td>
        <td>updates the number of replicas (temporary) yaml definition is not update</td>
    </tr>
    <tr>
        <td>Kubectl edit replicaset {name}</td>
        <td>you can edit the amount of replicas this way too (temporary)</td>
    </tr>
</table>