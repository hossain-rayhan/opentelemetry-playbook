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

## Regexp matching rules
**Empty-String:** `^$` or `^\\s$`

]If you need to check if a string contains only whitespaces, you can use `^\s*$`. Note that `\s` is the shorthand for the whitespace character class.

**Non-empty or Non-blank String:** `(.|\\s)*\\S(.|\\s)*`

This matches any string containing at least one non-whitespace character (the `\S` in the middle). It can be preceded and followed by anything, any character or whitespace sequence (including new lines): `(.|\s)*.`

N.B.: If you use raw string in go_test.go code then use single back slash, 
```
regexp.MustCompile(`(.|\s)*\S(.|\s)*`)
```

```yaml
-   include: MemoryUtilized
    action: insert
    new_name: MemoryUtilizedNew
    match_type: regexp
    match_labels: {"Type": "(.|\\s)*\\S(.|\\s)*", "container": "^$"} 
    operations:
        - action: add_label
          new_label: NewLabel
          new_value: Generated
```

**Understanding CPU limit and Request**

It depends on the what you are using to run containers. In kubernetes, `container_spec_cpu_quota` maps to container limits, and `container_spec_cpu_shares` is based on container requests.

If you wanted to know the percentage of cpu a container was using, you should be able to do something like `container_cpu_usage_seconds_total`/`container_spec_cpu_quota` * `some constant`. 

https://github.com/google/cadvisor/issues/2026#issuecomment-415571557


**Kubernetes Service commands**

Kubectl Service Commands:
```
a483e7119b0b:Desktop hrayhan$ kubectl get svc -A
NAMESPACE                    NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
adot-col3                    adot-collector           NodePort    10.100.213.232   <none>        8888:30522/TCP   19d
default                      kubernetes               ClusterIP   10.100.0.1       <none>        443/TCP          119d
fargate-container-insights   app-service-cluster-ip   ClusterIP   10.100.67.232    <none>        8888/TCP         2m4s
kube-system                  kube-dns                 ClusterIP   10.100.0.10      <none>        53/UDP,53/TCP    119d
monitoring                   fluent-bit               ClusterIP   None             <none>        2020/TCP         85d
```

```
a483e7119b0b:Desktop hrayhan$ kubectl get ep app-service-cluster-ip -n fargate-container-insights
NAME                     ENDPOINTS             AGE
app-service-cluster-ip   192.168.144.65:8888   3m47s
```

```
a483e7119b0b:Desktop hrayhan$ kubectl get services -o=wide -A
NAMESPACE                    NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE    SELECTOR
adot-col3                    adot-collector           NodePort    10.100.213.232   <none>        8888:30522/TCP   19d    component=adot-collector
default                      kubernetes               ClusterIP   10.100.0.1       <none>        443/TCP          119d   <none>
fargate-container-insights   app-service-cluster-ip   ClusterIP   10.100.67.232    <none>        8888/TCP         11m    component=adot-collector
kube-system                  kube-dns                 ClusterIP   10.100.0.10      <none>        53/UDP,53/TCP    119d   k8s-app=kube-dns
monitoring                   fluent-bit               ClusterIP   None             <none>        2020/TCP         85d    k8s-app=fluent-bit
```