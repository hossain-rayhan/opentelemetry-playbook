## EKS/Kubernetes basic commands
- `kubectl config view`
- `kubectl config current-context`
- `kubectl config use-context test-user@my-cluster.us-east-2.eksctl.io`

- `kubectl apply -f k8scluster-far.yaml`
- `kubectl get pods -A`
- `kubectl logs -n adot-col adot-collector-0`
- `kubectl get events --sort-by=.metadata.creationTimestamp -n adot-col`

- `kubectl replace —force -f k8scluster-far.yaml`
- `kubectl delete -f k8scluster-far.yaml`