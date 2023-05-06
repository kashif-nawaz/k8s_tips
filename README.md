# k8s_tips 

## Access kube-api via curl 
[Reference Link](https://kubernetes.io/docs/tasks/administer-cluster/access-cluster-api/)

```bash
kubectl proxy --port=8080 &
curl http://localhost:8080/api/
```
## Delete a namespace Stuck in Terminating State 

[Reference Link](https://www.redhat.com/sysadmin/troubleshooting-terminating-namespaces)

```bash
NAMESPACE=dev-ns
kubectl get namespace ${NAMESPACE} -o json > tmp.json

vim tmp.json

 "spec": {
        "finalizers": [
            "kubernetes"
        ]
    },

remove  "kubernetes" from above

"spec": {
        "finalizers": [
        ]
    },


curl -k -H "Content-Type: application/json" -X PUT --data-binary @tmp.json http://127.0.0.1:8080/api/v1/namespaces/${NAMESPACE}/finalize
```
## Delete PODs Stuck in Terminating State 
[Reference Link](https://lepczynski.it/en/k8s_en/k8s-pods-stuck-on-terminating/)

```bash
namespace="enter_the_name"

delpods=$(kubectl get pods -n ${namespace} | grep -i 'Terminating' | awk '{print $1 }')

for i in ${delpods[@]}; do
  kubectl delete pod $i --force=true --wait=false --grace-period=0  -n ${namespace}
done
```
