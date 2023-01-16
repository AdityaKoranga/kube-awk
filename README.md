# kube-awk
## AWK with Kubectl 
```bash
kubectl get pods -o wide | awk '{print $1}'
```
first column before spaces will be printed.

```bash
kubectl describe pod <pod_name> | awk ‘/Node:/ {print $2}’ 
```
This command will retrieve the detailed information about a specific pod and use awk to extract the name of the node where the pod is running

```bash
kubectl get services -o wide | awk ‘$2 == “NodePort” {print $1}’
```
```bash
kubectl get pods -o wide | awk ‘$2 == “0/1” {print $1}’
```

```bash
kubectl get pods -n <namespace> --no-headers=true | awk ‘/nginx-*|app-*/{print $1}’ | xargs kubectl delete -n grafana pod
```
This command will delete all the pods in a certain namespace following a certain patterns which will be in our case “nginx-* and app-*”, so any pods containing these pattern in their names, will be deleted even if these patterns are in the middle of names of the pods.

Note here that xargs is used to cascade output from awk to kubectl delete command.

```bash
kubectl get pods -n <namespace> --no-headers=true | awk '/^nginx-|^app-/{print $1}'
```
So at this point, we have a full list with the pods that can be deleted. Have a look to make sure the output is matching the expected one. Then safely apply the whole command.
