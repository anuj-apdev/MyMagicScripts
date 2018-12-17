# This Page contains some very handy Scripts that can be used in kubernetes

### To Delete all pods from a Node (Cordon)

```sh
kubectl get pods --all-namespaces \
   -o jsonpath='{range .items[?(.spec.nodeName=="gke-cluster-1-b4c97d4d-node-psh2")]}{@.metadata.namespace} {.metadata.name} {end}' |\
   xargs -n 2 | xargs -I % sh -c "kubectl delete pods --namespace=%"
pod "busybox-694uu" deleted
pod "fluentd-cloud-logging-gke-cluster-1-b4c97d4d-node-psh2" deleted
```

### Simples Kubernetes Custom Scheduler
```sh
#!/bin/bash
SERVER='localhost:8001'
while true;
do
    for PODNAME in $(kubectl --server $SERVER get pods -o json | jq '.items[] | select(.spec.schedulerName == "my-scheduler") | select(.spec.nodeName == null) | .metadata.name' | tr -d '"');
    do
        NODES=($(kubectl --server $SERVER get nodes -o json | jq '.items[].metadata.name' | tr -d '"'))
        NUMNODES=${#NODES[@]}
        CHOSEN=${NODES[$[$RANDOM % $NUMNODES]]}
        curl --header "Content-Type:application/json" --request POST --data '{"apiVersion":"v1", "kind": "Binding", "metadata": {"name": "'$PODNAME'"}, "target": {"apiVersion": "v1", "kind" : "Node", "name": "'$CHOSEN'"}}' http://$SERVER/api/v1/namespaces/default/pods/$PODNAME/binding/
        echo "Assigned $PODNAME to $CHOSEN"
    done
    sleep 1
done
```

