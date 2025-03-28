# Kubernetes Commands Cheat Sheet ☸️

A quick reference guide for Kubernetes commands, optimized for developers and DevOps.

---

## 🔹 Cluster Management

| Command | Description |
|---------|-------------|
| `kubectl cluster-info` | Display cluster info |
| `kubectl config view` | Show merged kubeconfig |
| `kubectl get nodes` | List all nodes |
| `kubectl describe node <node>` | Inspect a node |
| `kubectl top node` | Show node resource usage |
| `kubectl cordon <node>` | Mark node as unschedulable |
| `kubectl uncordon <node>` | Mark node as schedulable |
| `kubectl drain <node> --ignore-daemonsets` | Drain node for maintenance |

---

## 🔹 Namespace Operations

| Command | Description |
|---------|-------------|
| `kubectl get ns` | List namespaces |
| `kubectl create ns <name>` | Create namespace |
| `kubectl config set-context --current --ns=<name>` | Switch namespace |
| `kubectl delete ns <name>` | Delete namespace |

---

## 🔹 Pod Management

| Command | Description |
|---------|-------------|
| `kubectl get pods -A` | List all pods (all namespaces) |
| `kubectl get pods -n <ns>` | List pods in namespace |
| `kubectl describe pod <pod>` | Show pod details |
| `kubectl logs <pod>` | Print pod logs |
| `kubectl logs -f <pod>` | Stream pod logs |
| `kubectl exec -it <pod> -- /bin/sh` | Interactive shell into pod |
| `kubectl delete pod <pod>` | Delete a pod |
| `kubectl top pod` | Show pod resource usage |

---

## 🔹 Deployment & Scaling

| Command | Description |
|---------|-------------|
| `kubectl get deployments` | List deployments |
| `kubectl scale deploy <name> --replicas=3` | Scale deployment |
| `kubectl rollout status deploy/<name>` | Check rollout status |
| `kubectl rollout history deploy/<name>` | View rollout history |
| `kubectl rollout undo deploy/<name>` | Rollback deployment |
| `kubectl autoscale deploy <name> --min=2 --max=5 --cpu-percent=80` | Create HPA |

---

## 🔹 Service & Networking

| Command | Description |
|---------|-------------|
| `kubectl get svc` | List services |
| `kubectl expose deploy <name> --port=80 --target-port=8080` | Create service |
| `kubectl get endpoints` | List service endpoints |
| `kubectl get ing` | List ingress rules |
| `kubectl port-forward svc/<name> 8080:80` | Port-forward service |

---

## 🔹 ConfigMaps & Secrets

| Command | Description |
|---------|-------------|
| `kubectl create cm <name> --from-file=config.properties` | Create ConfigMap from file |
| `kubectl create secret generic <name> --from-literal=key=value` | Create secret |
| `kubectl get cm` | List ConfigMaps |
| `kubectl get secrets` | List secrets |
| `kubectl describe secret <name>` | Inspect secret |

---

## 🔹 Stateful Workloads

| Command | Description |
|---------|-------------|
| `kubectl get pv` | List persistent volumes |
| `kubectl get pvc` | List persistent volume claims |
| `kubectl get statefulsets` | List statefulsets |
| `kubectl describe pvc <name>` | Inspect PVC |

---

## 🔹 Troubleshooting

| Command | Description |
|---------|-------------|
| `kubectl get events --sort-by=.metadata.creationTimestamp` | Show cluster events |
| `kubectl describe <resource> <name>` | Detailed resource inspection |
| `kubectl api-resources` | List all API resources |
| `kubectl get --raw /metrics` | View raw metrics |
| `kubectl debug <pod> -it --image=busybox` | Debug pod (Ephemeral Container) |

---

## 🔹 YAML Operations

| Command | Description |
|---------|-------------|
| `kubectl apply -f file.yaml` | Create/update resources |
| `kubectl delete -f file.yaml` | Delete resources |
| `kubectl get <resource> -o yaml` | Get YAML manifest |
| `kubectl explain <resource>` | Show resource specification |
| `kubectl diff -f file.yaml` | Preview changes |

---

## 🔹 Advanced Commands

| Command | Description |
|---------|-------------|
| `kubectl kustomize <dir>` | Build kustomization |
| `kubectl auth can-i create pods` | Check permissions |
| `kubectl get --raw /apis` | List all APIs |
| `kubectl label pods <pod> env=prod` | Add label |
| `kubectl taint nodes <node> key=value:NoSchedule` | Add taint |

---

## 📝 Example Workflows

**Deploy an app:**

`kubectl create deployment nginx --image=nginx:latest --replicas=2`
`kubectl expose deployment nginx --type=LoadBalancer --port=80 --target-port=80 --name=nginx-service`
`kubectl scale deploy nginx --replicas=3`



## 🔹 Debugging & Diagnostics

| Command | Description |
|---------|-------------|
| `kubectl describe pod <pod>` | Show full pod details (events, state, etc.) |
| `kubectl logs <pod> --previous` | View logs from crashed container |
| `kubectl logs <pod> -c <container>` | View logs for specific container |
| `kubectl debug -it <pod> --image=busybox --target=<pod>` | Debug pod with ephemeral container |
| `kubectl get events --field-selector involvedObject.name=<pod>` | Filter events for specific pod |
| `kubectl top pod --containers` | Show CPU/Memory per container |
| `kubectl cp <pod>:/path/to/file ./local-file` | Copy file from pod |
| `kubectl auth can-i <verb> <resource>` | Check RBAC permissions |


## ☸️ Kubernetes Service Exposure  (Imperative Commands)
### 🔹 Basic Service Exposure
| Command | Description |
|---------|-------------|
| `kubectl expose deployment my-app --port=80 --target-port=8080` | ClusterIP (internal-only) |
| `kubectl expose deployment my-app --type=NodePort --port=80 --target-port=8080` | NodePort (external access via node IP) |
| `kubectl expose deployment my-app --type=LoadBalancer --port=80 --target-port=8080` | LoadBalancer (cloud providers) |
| `kubectl create service externalname external-db --external-name=legacy.example.com` | ExternalName (DNS CNAME) |
| `kubectl create service clusterip headless-svc --clusterip="None" --tcp=80:8080` | Headless Service (direct pod DNS) |
| `kubectl expose deployment my-app --type=NodePort --port=80 --target-port=8080 --name=my-service --node-port=31000` | Custom NodePort range (30000-32767) |
| <pre>`kubectl expose deployment my-app --type=LoadBalancer --port=80 --target-port=8080 --name=my-nlb`<br>`kubectl annotate svc my-nlb service.beta.kubernetes.io/aws-load-balancer-type="nlb"`</pre> | Cloud-specific LoadBalancer (AWS NLB example) |
| `kubectl expose deployment my-app --type=LoadBalancer --port=80 --target-port=8080 --name=multi-port --port=443 --target-port=8443` | Multiple ports exposure |
| `kubectl expose deployment my-app --type=LoadBalancer --port=80 --dry-run=client -o yaml` | Generate YAML without applying (dry-run) |
| `kubectl patch svc my-app -p '{"spec":{"externalTrafficPolicy":"Local"}}'` | Preserve client IP (LoadBalancer) |
| `kubectl annotate svc my-app service.beta.kubernetes.io/load-balancer-source-ranges="192.0.2.0/24,203.0.113.0/24"` | Restrict LoadBalancer source IPs (AWS example) |



# Advanced Debugging

**Capture pod state for analysis**

`kubectl get pod <pod> -o yaml > pod-state.yaml`

`kubectl logs <pod> > pod-logs.log`

**Network troubleshooting**

`kubectl run net-debug --image=nicolaka/netshoot --rm -it -- /bin/bash`


**Backup ETCD (Cluster State):**

`ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key  snapshot save /tmp/etcd-backup.db`

**Verify backup**

`etcdctl snapshot status /tmp/etcd-backup.db`
