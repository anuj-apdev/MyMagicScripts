Table of Content
=================

- [My favourite Aliases](#my-favourite-aliases)
- [Alertmanager updates on prometheus operator](#alertmanager-updates-on-prometheus-operator)
- [kubectl commands with advanced filters](#kubectl-commands-with-advanced-filters)
- [Create multiple YAML objects from stdin](#create-multiple-yaml-objects-from-stdin)
- [Some other kubectl Magic](#some-other-kubectl-magic)
- [Virtually hog memory to test memory alerts](#virtually-hog-memory-to-test-memory-alerts)
- [Quickly set proxy on VM with no Vim](#quickly-set-proxy-on-vm-with-no-vim)
- [init container for Checking Whether grafana is up or not](#init-container-for-checking-whether-grafana-is-up-or-not)
- [Job which seeds Grafana Dashboard](#job-which-seeds-grafana-dashboard)
- [Create metric using file based exporter](#create-metric-using-file-based-exporter)
- [Searching a User in LDAP](#searching-a-user-in-ldap)

## My favourite Aliases
```sh
alias k=kubectl
alias ks="kubectl -n kube-system"
kn(){ kubectl -n $ns "$@";}
ka(){ kubectl "$@" --all-namespaces;}
export KUBECONFIG=~/kube-config/devcluster.conf 
```
## Alertmanager updates on prometheus operator
```sh
kn get secret alertmanager-main -o "jsonpath={.data['alertmanager\.yaml']}" | base64 -D

kn create secret generic alertmanager-main --from-literal=alertmanager.yaml="$(< alertmanager.yaml)" --dry-run -oyaml | 
kn replace secret --filename=-
```
## kubectl commands with advanced filters 
```sh
# Sort output of get pod command
kubectl get po --all-namespaces -owide --sort-by=.metadata.creationTimestamp

# Get Literal values inside a secret
kubectl -n monitoring get secrets blackbox-scrape-configs -o go-template='{{ range $k, $v := .data }}{{ $v | base64decode}}{{"\n"}}{{end}}'

# To get All Services with Nodeport Values
kubectl get --all-namespaces svc -o json | jq -r '.items[] | [.metadata.name,([.spec.ports[].nodePort | tostring ] | join("|"))] | @csv'

# Get all ServiceAccounts
kubectl get rolebindings --all-namespaces -o go-template='{{range .items}}{{println}}{{range .subjects}}{{if eq .kind "ServiceAccount"}}{{.namespace}}::{{.name}} {{end}}{{end}}{{end}}'

# For each pod, list whether it containers run on a read-only root filesystem or not:
# This is a one lines security check, All pods having write access to root filesystem might cause security issues
kubectl get pods --all-namespaces -o go-template --template='{{range .items}}{{.metadata.name}}{{"\n"}}{{range .spec.containers}}    read-only: {{if .securityContext.readOnlyRootFilesystem}}{{printf "\033[32m%t\033[0m" .securityContext.readOnlyRootFilesystem}} {{else}}{{printf "\033[91m%s\033[0m" "false"}}{{end}} ({{.name}}){{"\n"}}{{end}}{{"\n"}}{{end}}'

# Getting list of "not running" pods
kubectl get po -a --all-namespaces -o json --field-selector="status.phase!=Running"

# Restarting(Deleting) not running pods
kubectl get po -a --all-namespaces -o json --field-selector="status.phase!=Running" | jq  '.items[]  | "kubectl delete po \(.metadata.name) --n \(.metadata.namespace)"' | xargs -n 1 bash -c

#Get sorted list of nodes with respect to Memory Capacity.
kubectl get no -o json | jq -r '.items | sort_by(.status.capacity.memory)[]|[.metadata.name,.status.capacity.memory]| @tsv'

# Get pod on specific Node
node=node3
kubectl get po --all-namespaces  -o json | jq -r '.items | sort_by(.spec.nodeName)[]|select(.spec.nodeName=="$node")|[.metadata.name,.spec.nodeName]| @tsv'

# List PV's with storage capacity and details
kubectl get pv -o json | jq -r '.items | sort_by(.spec.capacity.storage)[]|[.metadata.name,.spec.capacity.storage]| @tsv'


```
## Create multiple YAML objects from stdin
```sh
cat <<EOF | kubectl create -f -
apiVersion: v1
kind: Pod
metadata:
  name: busybox-sleep
spec:
  containers:
  - name: busybox
    image: busybox
    args:
    - sleep
    - "1000000"
---
apiVersion: v1
kind: Pod
metadata:
  name: busybox-sleep-less
spec:
  containers:
  - name: busybox
    image: busybox
    args:
    - sleep
    - "1000"
EOF

## Create a secret with several keys
cat <<EOF | kubectl create -f -
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  password: $(echo -n "s33msi4" | base64 -w0)
  username: $(echo -n "jane" | base64 -w0)
EOF
```

## Some other kubectl Magic
```sh
# Run deployment
$ kubectl run nginx --replicas=2 --image=nginx
# Expose Deployment , i.e. create svc
$ kubectl expose deployment nginx --port=80
# Test by running a different Pod
$ kubectl run access --rm -ti --image busybox /bin/sh
# Access the service 
$ wget -q nginx -O -
```

## Virtually hog memory to test memory alerts
```sh
dd if=/dev/zero of=/tmp/hd-fillup.zeros bs=1G count=50
fallocate -l 50G file 
fallocate -l 50G big_file
truncate -s 50G big_file
dd of=bigfile bs=1 seek=50G count=0
df -h . ; dd of=biggerfile bs=1 seek=5000G count=0 ; ls -log biggerfile ; df -h .
memhog
```
### Checking free space on device
```sudo find /sys/fs/cgroup/memory | wc -l 2946
'''

## Quickly set proxy on VM with no Vim
```sh
cat > /etc/apt/apt.conf << EOF
Acquire::http::proxy "http://www-proxy.us.oracle.com:80/";
Acquire::https::proxy "https://www-proxy.us.oracle.com:80/";
Acquire::ftp::proxy "ftp://www-proxy.us.oracle.com:80/";
EOF
```

## init container for Checking Whether grafana is up or not

```sh
echo \"waiting for endpoints...\"; 
while true; 
	set endpoints \
		$(curl -s --cacert /var/run/secrets/kubernetes.io/serviceaccount/ca.crt \
		--header \"Authorization: Bearer \"(cat /var/run/secrets/kubernetes.io/serviceaccount/token) \
		https://kubernetes.default.svc/api/v1/namespaces/monitoring/endpoints/grafana); 

	echo $endpoints | jq \".\"; 

	if test \
		(echo $endpoints | jq -r \".subsets[]?.addresses // [] | length\") -gt 0; 
		exit 0; 
	end; 

	echo \"waiting...\";
	sleep 1; 
end"
```

## Job which seeds Grafana Dashboard

```sh
for file in *-datasource.json ; do
  if [ -e "$file" ] ; then
    echo "importing $file" &&
    curl --silent --fail --show-error \
      --request POST http://admin:admin@grafana:3000/api/datasources \
      --header "Content-Type: application/json" \
      --data-binary "@$file" ;
    echo "" ;
  fi
done ;

for file in *-dashboard.json ; do
  if [ -e "$file" ] ; then
    echo "importing $file" &&
    ( echo '{"dashboard":'; \
      cat "$file"; \
      echo ',"overwrite":true,"inputs":[{"name":"DS_PROMETHEUS","type":"datasource","pluginId":"prometheus","value":"prometheus"}]}' ) \
    | jq -c '.' \
    | curl --silent --fail --show-error \
      --request POST http://admin:admin@grafana:3000/api/dashboards/import \
      --header "Content-Type: application/json" \
      --data-binary "@-" ;
    echo "" ;
  fi
done
```

## Create metric using file based exporter
```sh
touch /tmp/metrics-temp
while true
  for directory in (du --bytes --separate-dirs --threshold=100M /mnt)
    echo $directory | read size path
    echo "node_directory_size_bytes{path=\"$path\"} $size" \
      >> /tmp/metrics-temp
  end
  mv /tmp/metrics-temp /tmp/metrics
  sleep 300
end
```

## Searching a User in LDAP
```sh
ldapsearch -o ldif-wrap=no -h ldap.myorg.com -z 0 -S cn -LLL -x -b "dc=myorg,dc=com" "phonenumber=1206xxxzzzz"
```
