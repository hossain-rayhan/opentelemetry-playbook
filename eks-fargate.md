## EKS/Kubernetes basic commands
Config:
- `kubectl config view`
- `kubectl config current-context`
- `kubectl config use-context test-user@my-cluster.us-east-2.eksctl.io`

Deploy and logs:
- `kubectl apply -f k8scluster-far.yaml`
- `kubectl get pods -A`
- `kubectl logs -n adot-col adot-collector-0`
- `kubectl get events --sort-by=.metadata.creationTimestamp -n adot-col`

Replace or delete:
- `kubectl replace —force -f k8scluster-far.yaml`
- `kubectl delete -f k8scluster-far.yaml`

**Get raw metrics from Kubelets cAdvisor endpoint:**

`kubectl get --raw "/api/v1/nodes/fargate-ip-192-168-137-185.us-east-2.compute.internal/proxy/metrics/cadvisor"`