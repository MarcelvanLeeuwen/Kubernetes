# Kubectl

<table>
    <tr>
        <th>Command</th>
        <th>Explanation</th>
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
    </tr>
    <tr>    
        <td>kubectl create deployment —image=nginx --dry-run=client -o yaml</td>
    </tr>
    <tr>
        <td>kubectl create deployment —image=nginx --replicas=4 --dry-run=client -o yaml > nginx.yaml</td>
    </tr>
    <tr>
        <td>kubectl create -f {filename.yaml} --> create’s a kubernetes resource from a yaml file</td>
    </tr>
    <tr>
        <td>kubectl delete pod, replicaset, service, deployment</td> 
    </tr>
    <tr>
        <td>kubectl get pods, deployment, service, replicaset</td>
    </tr>
    <tr>
        <td>kubectl get all</td>
    </tr>
    <tr>
        <td>kubectl create -f {filename.yaml} --> this can be services, replicasets, deployments</td>
    </tr>
    <tr>
        <td>kubectl scale —replicas=6 -f {file.yaml}</td>
        <td>updates the number of replicas / is temporary because the yaml definition is not update</td>
    </tr>
    <tr>
        <td>Kubectl edit replicaset {name}</td>
        <td>you can edit the amount of replicas this way too, but it is temporary. Edit yaml file if you want to make it persistent</td>
    </tr>
</table>